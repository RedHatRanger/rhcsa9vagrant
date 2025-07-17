## UPDATED 2025-03-13:


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
```

### Step 2: Prepare Disks with parted
Execute these commands for each new disk (`/dev/vdb`, `/dev/vdc`, `/dev/vdd`):
```bash
[root@node2 ~]# parted /dev/vdb mklabel gpt
[root@node2 ~]# parted -s /dev/vdb mkpart primary xfs 0% 100%
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
[root@node2 ~]# echo "/dev/VG1/LV1 /lv xfs defaults,nodev 0 0" >> /etc/fstab
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
DexTutor's tutorial can be found <a href="https://www.youtube.com/watch?v=N3HFDvV-d-w">here</a>
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #15.3:

**Step 1: Check Current Volume Group Configuration**

```bash
[root@node2 ~]# vgdisplay
```

**Step 2: If current PE size isn't 8MB, recreate the volume group:**

```bash
[root@node2 ~]# umount -l /lv
[root@node2 ~]# lvchange -an /dev/VG1/LV1
[root@node2 ~]# lvremove /dev/VG1/LV1
[root@node2 ~]# vgremove VG1
```

**Step 3: Create a new Volume Group with 8MB extents:**

```bash
[root@node2 ~]# vgcreate -s 8MB VG1 /dev/vdb
[root@node2 ~]# vgdisplay VG1
```

**Step 4: Create Logical Volume `LV2` with 10 extents:**

```bash
[root@node2 ~]# lvcreate -l 10 -n LV2 VG1
[root@node2 ~]# lvs
```

Your Logical Volume `LV2` is now set up successfully with custom extent size.

* SUCCESS!!

<br><br><br>
### Troubleshooting:
Remove existing configuration if required and recreate:
```bash
[root@node2 ~]# umount -l /lv
[root@node2 ~]# lvchange -an /dev/VG1/LV1
[root@node2 ~]# lvremove /dev/VG1/LV1
[root@node2 ~]# vgremove VG1
[root@node2 ~]# vgcreate -s 8MB VG1 /dev/vdb
[root@node2 ~]# vgdisplay VG1
```

---

## Conclusion
You've successfully configured Logical Volume Management using parted, managing and adjusting logical volumes as required.
