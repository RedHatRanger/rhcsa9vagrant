**On Node2**

# Create whole LVM stack

### QUESTION #15:
QUESTION #15: 
Create an LVM name wshare from wgroup volume group. Note the following: \
    - PE size should be 8MB \
    - LVM size should be 50 extents \
    - Format with "ext4" file system and mount it under /mnt/wshare. And it should auto mount after next reboot \
    - (Exam may have you setup filesystem - vfat, xfs) \

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #15:

* First, if you are running virtual machines, you will need to add an additional storage device here...5GB...
* You can shut down the virtual machine and then add the additional storage as needed.

* You can issue:

```
[root@node2 ~]# lsblk
NAME           MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS
sro              11:0    1 1024M  0 rom
vda            252:0    0   10G  0 disk
├─vda1         252:1    0    1G  0 part /boot
├─vda2         252:2    0    9G  0 part
│ ├─_rhe1_192-root 253:0 0  8G  0 lvm  /
│ └─_rhe1_192-swap 253:1 0  1G  0 lvm  [SWAP]
vdb            252:16   0    5G  0 disk
[root@node2 ~]#
```

* Now that we have our target “vdb” we will run “fdisk /dev/vdb” to format it: 
```
[root@node2 ~]# fdisk /dev/vdb

Welcome to fdisk (util-linux 2.37.4).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table.
Created a new DOS disklabel with disk identifier 0x04aee29f.

Command (m for help): 
```

* We will type "n", then "p", and then "+500M" (we get this by multiplying 8 x 50):
```
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p):
Using default response p.
Partition number (1-4, default 1):
First sector (2048-10485759, default 2048): 2048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-10485759, default 10485759): +500M
Created a new partition 1 of type 'Linux' and of size 500 MiB.
```
* Change the type to 8E for Linux LVM partitions:
```
Command (m for help): t
Selected partition 1
Hex code or alias (type L to list all): 82
Changed type of partition 'Linux' to 'Linux swap / Solaris'.
```
* Lastly, type "w" to write the partition to the drive:
```
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.
```
* Lastly, don't forget to run "partprobe /dev/vdb" to update the File Table:
```
[root@node2 ~]# partprobe /dev/vdb
```
* As a test, you can run:
```
[root@node2 ~]# fdisk /dev/vdb -l
Disk /dev/vdb: 5 GiB, 5368709120 bytes, 10485760 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x04aee29f

Device     Boot Start     End Sectors Size Id Type
/dev/vdb1        2048 1026047 1024000 500M 8e Linux LVM
```

* When we got the partition the first step is to create **physical partition**: 

```
[root@node2 ~]# pvcreate /dev/vdb1
  Physical volume "/dev/vdb1" successfully created.
```
```
[root@node2 ~]# vgcreate -s 8M wgroup /dev/vdb1
  Volume group "wgroup" successfully created
```
```
[root@node2 ~]# lvcreate -l 50 -n wshare /dev/wgroup
  Logical volume "wshare" created.
```
* To confirm you can type "lvs" to view the LVM you created:
```
[root@node2 ~]# lvs
LV     VG      Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
root   rhel_192 -wi-ao---- <8.00g                                                    
swap   rhel_192 -wi-ao---- 1.00g                                                    
wshare wgroup  -wi-a----- 200.00m
```
* Don’t forget to format using ```mkfs.ext4 /dev/wgroup/wshare```:
```
[root@node2 ~]# mkfs.ext4 /dev/wgroup/wshare
mke2fs 1.46.5 (30-Dec-2021)
Discarding device blocks: done                            
Creating filesystem with 409600 1k blocks and 102400 inodes
Filesystem UUID: dc2bedb3-e528-4c92-8ea3-a3c0619d769f
Superblock backups stored on blocks: 
  8193, 24577, 40961, 57345, 73729, 204801, 221185, 401409
Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (8192 blocks): done
Writing superblocks and filesystem accounting information: done   
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
/dev/wgroup/wshare       /mnt/wshare              ext4    defaults        0 0


:wq
```
* Run ```mount -a``` to mount the /etc/fstab mounts:
```
[root@node2 ~]# mount -a
[root@node2 ~]# df -h
Filesystem                      Size  Used Avail Use% Mounted on
devtmpfs                        867M    0  867M   0% /dev
tmpfs                           887M    0  887M   0% /dev/shm
tmpfs                           355M  5.1M  350M   2% /run
/dev/mapper/rhel_192-root       8.0G  1.6G  6.5G  20% /
/dev/vda1                       1014M  220M  795M  22% /boot
tmpfs                           178M    0  178M   0% /run/user/0
/dev/mapper/wgroup-wshare       365M   14K  341M   1% /mnt/wshare
```

* SUCCESS!!
