**On Node2**

# Create whole LVM stack

### QUESTION #15:
QUESTION #15: 
Create an LVM name wshare from wgroup volume group. Note the following: 
    - PE size should be 8MB 
    - LVM size should be 50 extents 
    - Format with "ext4" file system and mount it under /mnt/wshare. And it should auto mount after next reboot 
    - (Exam may have you setup filesystem - vfat, xfs) 

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
* Change the type to 8E for Linux LVM partitions (he made a mistake and had to type "8E"):
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
# Don't forget to run "partprobe /dev/vdb" to update the File Table:
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

# Note: if the output for label/partition table contains **DOS** in any way then the partition table was configured using **MBR**. 
If there is anything else - **GPT**. **It is crucial to not mix them on one device (creating MBR partitions on GPT and vice versa)!**



* When we got the partition the first step is to create **physical partition**: 

```
pvcreate /dev/PARTITION_IDENTIFIER
# check if it was created
pvdisplay
```

* Physical partition can be assigned to the **volume group**.

```
vgcreate -s 16M datacontainer /dev/PARTITION_IDENTIFIER
# check if it was created
vgdisplay
```

* Now it is time to create **logical volume** (notice **-l switch** that allows specifying extents number):

```
lvcreate -l 50 -n datacopy datacontainer
# check if it was created
lvdisplay
```

* Creating filesystem can be done with **mkfs** with **type** flag or with specialized tool like below:

```
mkfs.vfat /dev/datacontainer/datacopy
```

* Now we create mount point for it and we provide entry in **fstab** file to make it permanent:

```
mkdir /datasource
# get UUID for created logical volume
blkid
# edit /etc/fstab and append there below line
UUID=UUUID_IDENTIFIER_COPIED_FROM_BLKID  /datasource vfat  defaults   0 0    
```

* Save the file and check if everything works:

```
mount -a
```
