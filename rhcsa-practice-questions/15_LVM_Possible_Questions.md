## UPDATED 2025-03-10:


<br><br>
***On Node2***

# Lab #15: Logical Volume Management (LVM)

## Objective:
Create and manage Logical Volumes using parted

### QUESTION #15.1:
Create a Logical Volume named LV1 of size 8GB using the extra storage provided.  Here is what you will learn:
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/47206a5d-73f3-483e-b9d6-e3f35aea26ab)

DexTutor's tutorial can be found <a href="https://www.youtube.com/watch?v=N3HFDvV-d-w">here</a>
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #15.1:

* First, if you are running virtual machines, you will need to add an additional storage device here...6GB,5GB, and 4GB "Hard Disks"
* You can shut down the virtual machine and then add the additional storage as needed.

### Step 1: Verify Attached Storage
```bash
[root@node2 ~]# lsblk
NAME           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sro              11:0    1 1024M  0 rom
vda            252:0    0   10G  0 disk
├─vda1         252:1    0    1G  0 part /boot
├─vda2         252:2    0    9G  0 part
│ ├─_rhe1_192-root 253:0 0  8G  0 lvm  /
│ └─_rhe1_192-swap 253:1 0  1G  0 lvm  [SWAP]
vdb            252:16   0    6G  0 disk
vdc            252:17   0    5G  0 disk
vdd            252:18   0    4G  0 disk
[root@node2 ~]#
```

### Step 2: Prepare Disks with parted
Execute these commands for each new disk (`/dev/vdb`, `/dev/vdc`, `/dev/vdd`):
```bash
[root@node2 ~]# parted /dev/vdb mklabel gpt
[root@node2 ~]# parted -a opt /dev/vdb mkpart primary xfs 0% 100%
[root@node2 ~]# parted /dev/vdb set 1 lvm on
```
>Repeat similarly for `/dev/vdc` and `/dev/vdd`.

### Step 3: Verify Partitioning
```bash
lsblk
```

### Step 4: Create Physical Volumes
```bash
[root@node2 ~]# pvcreate /dev/vdb /dev/vdc /dev/vdd

# We can verify that they are available:
[root@node2 ~]# pvs
PU           VG      Fmt    Attr    PSize   PFree
/dev/sdb             1um2   ---     6.00g   6.00g
/dev/sdc             1vm2   ---     5.00g   5.00g
/dev/sdd             1um2   ---     4.00g   4.00g
```

### Step 5: Create Volume Group
```bash
[root@node2 ~]# vgcreate VG1 /dev/sdb /dev/sdc
  Volume group "VG1" successfully created

# Verify the new VG1 has been created:
[root@node2 ~]# vgs
VG   #PU  #LV #SN Attr     VSize   VFree
VG1  2    0   0   wz--n-   10.99g  10.99g
```

### Step 6: Create Logical Volume (LV1) using 8GB
```bash
[root@node2 ~]# lvcreate -L 8Gb -n LV1 VG1
  Logical volume "LV1" created.

# Verify:
[root@node2 ~]# lvs
LV     VG     Attr      LSize  Pool Origin Data Metax Move Log Cy Sync Convert
LV1    VG1    wi-a--    8.00g
```

### Step 7: Format and Mount Logical Volume
```bash
[root@node2 ~]# mkfs.xfs /dev/VG1/LV1
[root@node2 ~]# mkdir /lv
```

### Step 8: Automate Mounting with `/etc/fstab`
```bash
[root@node2 ~]# echo "/dev/VG1/LV1 /lv xfs defaults 0 0" >> /etc/fstab
```

### Step 9: Instantly mount the LVM without a reboot
```bash
[root@node2 ~]# mount -a
```

* SUCCESS!!

<br><br><br>
### QUESTION #15.2:
Extend the Logical Volume you created, LV1 by 2GB. 
<br/><br/>
Again, DexTutor's tutorial can be found <a href="https://www.youtube.com/watch?v=N3HFDvV-d-w">here</a>
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #15.2:

### Step 1: Extend Logical Volume by 2GB
Check available space first:
```bash
[root@node2 ~]# vgs
VG   #PV  #LV #SN   Attr     VSize   VFree
VG1  2    1   0     wz--n-   10.99g  2.99g
```

>If space permits on VG1 with the 2 physical disks:
```bash
[root@node2 ~]# lvextend -r -L +2G /dev/VG1/LV1

# Verify:
[root@node2 ~]# lvs
```

### Step 2: Extend Volume Group VG1 to /dev/sdd
If you need more space:
```bash
[root@node2 ~]# vgextend VG1 /dev/vdd
[root@node2 ~]# lvextend -r -L +2G /dev/VG1/LV1
```

### Step 3: Verify New Size
```bash
[root@node2 ~]# df -h
```

* SUCCESS!!

### QUESTION #15.3:
Create a Logical Volume named LV2 with 10 extent where the size of each extent is 8MB.
<br/><br/>
Again, DexTutor's tutorial can be found <a href="https://www.youtube.com/watch?v=N3HFDvV-d-w">here</a>
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #15.3:

* Let's first have a look at our volume group:
```
[root@node2 ~]# vdisplay
  --- Volume group ---
  VG Name               UG1
  System ID             
  Format                lvm2
  Metadata Areas        3
  Metadata Sequence No  5
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                3
  Cur PV                3
  Act PV                3
  VG Size               14.99 GiB
  PE Size               4.00 MiB
  Total PE              3837
  Alloc PE / Size       3072 / <12.88 GiB
  Free  PE / Size       765 / <2.99 GiB
  VG UUID               URDhJh-QMzL-mWJY-1PKT-0FNC-a2UB-A1XyvP
```

* By default, the PE or "Physical Extents" size is 4MB, but since the question is calling for extents to be 8MB in size:
```
[root@node2 ~]# umount -l /lv
[root@node2 ~]# lvchange -an /dev/VG1/LV1
[root@node2 ~]# lvremove /dev/VG1/LV1
  Logical volume "LV1" successfully removed
[root@node2 ~]# vgremove VG1
  Volume group "VG1" successfully removed
[root@node2 ~]# vgs
[root@node2 ~]#
[root@node2 ~]# vgcreate -s 8MB VG1 /dev/sda
  Volume group "VG1" successfully created
[root@node2 ~]# vgdisplay
  --- Volume group ---
  VG Name               UG1
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                1
  Cur PV                1
  Act PV                1
  VG Size               5.99 GiB
  PE Size               8.00 MiB
  Total PE              767
  Alloc PE / Size       0 / 0
  Free  PE / Size       767 / 5.99 GiB
  VG UUID               cmrSly-Yd61-aqua-mtTS-Fkcl-dN1b-NICrPJ
```

* Notice in the above output that the Extents have changed to 8MB (PE):



















### Troubleshooting:
Remove existing configuration if required and recreate:
```bash
umount -l /lv
lvchange -an /dev/VG1/LV1
lvremove /dev/VG1/LV1
vgremove VG1
vgcreate -s 8MB VG1 /dev/vdb
vgdisplay VG1
```

---

## Conclusion
You've successfully configured Logical Volume Management using parted, managing and adjusting logical volumes as required.
