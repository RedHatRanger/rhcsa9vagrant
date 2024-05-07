***On Node2***

# Create swap partition

### QUESTION #16: 
Create a swap partition of 400MB and make it available permanent. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #16:

* Before any kind of operations on ***LVM*** it is good to know what we actually have in the system. The proper command to list all
devices we can use is in order **pvs**, **vgs** and **lvs**. It shows all physical storages and devices, volume groups and logical volumes.

* First we create a logical volume that is mentioned in the question and we create swap on it:
```
[root@node2 ~]# fdisk /dev/vdb

Welcome to fdisk (util-linux 2.37.4).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
This disk is currently in use - repartitioning is probably a bad idea.
It's recommended to unmount all file systems, and swapoff all swap partitions on this disk.

Command (m for help): 
```
* Type “n”, “p”, and then “+400M”:

```
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 3 free)
   e   extended (container for logical partitions)
Select (default p):
Using default response p.
Partition number (1-4, default 2): 2
First sector (2048-10485759, default 1026048): 1026048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (1026048-10485759, default 10485759): +400M
Created a new partition 2 of type 'Linux' and of size 400 MiB.
```

* Change the partition: Hit "t" for type, "2" for partition 2, "82" for swap, then “w” to write it:

```
Command (m for help): t
Partition number (1,2, default 2): 2
Hex code or alias (type L to list all): 82
Changed type of partition 'Linux' to 'Linux swap / Solaris'.
```

* Don’t forget to run partprobe:
```
[root@node2 ~]# partprobe /dev/vdb
```
Format the new swap partition on vdb2, then add it to the main swap:
[root@node2 ~]# mkswap /dev/vdb2
Setting up swapspace version 1, size = 400 MiB (419426304 bytes)
no label, UUID=9abc60cf-9d66-4d3a-8ffe-218770ee99ad
```
[root@node2 ~]# swapon /dev/vdb2
```

* Type “blkid” to find the UUID #:
```
[root@node2 ~]# blkid | grep vdb2
/dev/vdb2: UUID="9abc60cf-9d66-4d3a-8ffe-218770ee99ad" TYPE="swap" PARTUUID="d0ae2e9f-02"
```

* To make swap changes permanent as usual ***/etc/fstab*** must be changed:
```
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
/dev/mapper/wgroup-wshare /mnt/wshare             ext4    defaults        0 0
UUID=9abc60cf-9d66-4d3a-8ffe-218770ee99ad swap    swap    defaults        0 0
```

```
[root@node2 ~]# systemctl daemon-reload
```

* Run “mount -a” to mount the swap partition:
```
[root@node2 ~]# mount -a
```

* Run the ```swapon -s``` command to add the swap volume to the main swap volume:
 
```
[root@node2 ~]# swapon -s
Filename                                Type        Size    Used    Priority
/dev/dm-1                               partition   1048572 0       -1
/dev/vdb2                               partition   409596  0       -2
```
* To check swap/memory statistics:
```
free -k
```

* SUCCESS!!


### Additional comment:

Both commands **mkswap** and **swapon** have parameters like **-L** and **-U** so it is possible to give these commands not only the
path (to the file or partition) but label or UUID.
