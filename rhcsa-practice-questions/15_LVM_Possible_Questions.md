***On Node2***

# Logical Volume Management

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

* Let's start by running ```lsblk``` to see what physically attached devices we have.

```
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

* The first step is to create the **physical volumes**: 
```
[root@node2 ~]# pvcreate /dev/vdb
  Physical volume "/dev/vdb1" successfully created.
[root@node2 ~]# pvcreate /dev/vdc
  Physical volume "/dev/vdc" successfully created.
[root@node2 ~]# pvcreate /dev/vdd
  Physical volume "/dev/vdd" successfully created.
```

* We can verify that they are available:
```
[root@node2 ~]# pvs
PU           VG      Fmt    Attr    PSize   PFree
/dev/sdb             1um2   ---     6.00g   6.00g
/dev/sdc             1vm2   ---     5.00g   5.00g
/dev/sdd             1um2   ---     4.00g   4.00g
```

* Next, we create the Volume Group ```LV1``` using two of our extra disks (11GB) because the 1st one was too small for 8GB:
```
[root@node2 ~]# vgcreate VG1 /dev/sdb /dev/sdc
  Volume group "VG1" successfully created
[root@node2 ~]# vgs
VG   #PU  #LV #SN Attr     USize   UFree
VG1  2    0   0   wz--n-   10.99g  10.99g
```

* Now, we create the Logical Volume:
```
[root@node2 ~]# lvcreate -L 8Gb -n LV1 VG1
  Logical volume "LV1" created.
```

* We can verify that the Logical Volume has been created:
```
[root@node2 ~]# lvs
LV     VG     Attr      LSize  Pool Origin Data Metax Move Log Cy Sync Convert
LV1    VG1    wi-a--    8.00g
```

* Next, we will create an auto mount point in /etc/fstab:
```
[root@node2 ~]# vim /etc/fstab

# /etc/fstab
# Created by anaconda on Fri Dec 30 16:18:12 2022
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
/dev/mapper/rhel_192-root /                       xfs     defaults        0 0
UUID=0efecb9e-7ecd-47e0-ad64-507f7c994e18 /boot  xfs     defaults        0 0
/dev/mapper/rhel_192-swap none                    swap    defaults        0 0
/dev/VG1/LV1             /lv                      xfs     defaults        0 0


:wq
```

* Create the mountpoint and then run ```mount -a``` to mount the /etc/fstab mounts:
```
[root@node2 ~]# mkdir /lv
[root@node2 ~]# mkfs.xfs /dev/VG1/LV1
meta-data=/dev/vg1/lv1 isize=512 agcount=4, agsize=524288 blks
         =                       sectsz=512 attr=2, projid32bit=1
         =                       crc=1 finobt=1, sparse=1, rmapbt=0
data     =                       bsize=4096 blocks=2097152, imaxpct=25
         =                       sunit=0 swidth=0 blks
naming   =version 2              bsize=4096 ascii-ci=0, ftype=1
log      =internal log           bsize=4096 blocks=2560, version=2
         =                       sectsz=512 sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096 blocks=0, rtextents=0
[root@node2 ~]# 
[root@node2 ~]# systemctl daemon-reload
[root@node2 ~]# mount -a
```

* SUCCESS!!








