***On Node2***

# Reduce the size of a Logical Volume (LV)

### QUESTION #17:
Resize your wshare logical volume you created in Lab #15, it should be approx 300MB ( note -> only size accepted from 270mb to 290mb).  

***Note: In the real world, there could be losses of data, so YOU MUST BACKUP YOUR DATA before doing this operation!***
Reducing the LVM may or may not be on the actual exam, but it is good to know how to do anyway.  You will more likely see LVEXTEND.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #17:

* Run ```lvs``` to check your logical volumes and then check wshare to see if it is mounted as "ext4":
```
[root@node2 ~]# lvs
LV      VG      Attr       LSize Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
root    rhel_192 -wi-ao---- <8.00g                                                    
swap    rhel_192 -wi-ao---- 1.00g                                                    
wshare  wgroup  -wi-a----- 400.00m                                                    
...
[root@node2 ~]#
[root@node2 ~]# df -h
Filesystem                        Size  Used Avail Use% Mounted on
devtmpfs                          867M     0  867M   0% /dev
tmpfs                             887M     0  887M   0% /dev/shm
tmpfs                             355M  5.1M  350M   2% /run
/dev/mapper/rhel_192-root         8.0G  1.6G  6.5G  20% /
/dev/vda1                         1014M  220M  795M  22% /boot
tmpfs                             178M     0  178M   0% /run/user/0
/dev/mapper/wgroup-wshare         365M   14K  341M   1% /mnt/wshare
```

* Unmount the /mnt/wshare, then run a filesystem check on the wgroup-wshare volume:
```
[root@node2 ~]# umount /mnt/wshare
[root@node2 ~]# e2fsck -f /dev/mapper/wgroup-wshare
e2fsck 1.46.5 (30-Dec-2021)
Pass 1: Checking inodes, blocks, and sizes
```

* Then run ```resize2fs``` on the volume:
```
[root@node2 ~]# resize2fs /dev/mapper/wgroup-wshare 300M
resize2fs 1.46.5 (30-Dec-2021)
Resizing the filesystem on /dev/mapper/wgroup-wshare to 307200 (1k) blocks.
The filesystem on /dev/mapper/wgroup-wshare is now 307200 (1k) blocks long.
```

* Then run ```lvreduce```:
```
[root@node2 ~]# lvreduce -L -100M -n /dev/mapper/wgroup-wshare
Rounding size to boundary between physical extents: 96.00 MiB.
WARNING: Reducing active logical volume to 304.00 MiB.
THIS MAY DESTROY YOUR DATA (filesystem etc.).
Do you really want to reduce wgroup/wshare? [y/n]: y
Size of Logical Volume wgroup/wshare changed from 400.00 MiB (50 extents) to 304.00 MiB (38 extents).
Logical volume wgroup/wshare successfully resized.
```

* Remount all partitions:
```
[root@node2 ~]# mount -a
```

* Recheck all the mounted partitions:
```
[root@node2 ~]# df -h
Filesystem                        Size    Used    Avail   Use%    Mounted on
devtmpfs                          867M    0       867M    0%      /dev
tmpfs                             887M    0       887M    0%      /dev/shm
tmpfs                             355M    5.1M    350M    2%      /run
/dev/mapper/rhel_192-root         8.0G    1.6G    6.5G    20%     /
/dev/vda1                         1014M   220M    795M    22%     /boot
tmpfs                             178M    0       178M    0%      /run/user/0
/dev/mapper/wgroup-wshare         273M    2.1M    252M    1%      /mnt/wshare
```

* To check your shrunken logical volume, re-run "lvs":
```
[root@node2 ~]# lvs
LV      VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
root    rhel_192 -wi-ao---- <8.00g                                                    
swap    rhel_192 -wi-ao---- 1.00g                                                    
wshare  wgroup  -wi-ao----  304.00m                                                    
```
* It should be around 300MB.....

* SUCCESS!!


# The Test may also ask you to extend the logical volume.  In that case:
* NOW THE REVERSE, resize it to 400MB:
```
[root@node2 ~]# df -Th
Filesystem                        Type    Size    Used    Avail   Use%    Mounted on
devtmpfs                          devtmpfs 867M    0       867M    0%      /dev
tmpfs                             tmpfs    887M    0       887M    0%      /dev/shm
tmpfs                             tmpfs    355M    5.1M    350M    2%      /run
/dev/mapper/rhel_192-root         xfs      8.0G    1.6G    6.5G    20%     /
/dev/vda1                         xfs      1014M   220M    795M    22%     /boot
tmpfs                             tmpfs    178M    0       178M    0%      /run/user/0
/dev/mapper/wgroup-wshare         ext4    273M    2.1M    252M    1%      /mnt/wshare
```

* Run ```lvextend -r -L +100M /dev/mapper/wgroup-wshare``` with the -r flag to resize the volume as you extend it:
```
[root@node2 ~]# lvextend -r -L +100M /dev/mapper/wgroup-wshare
Rounding size to boundary between physical extents: 104.00 MiB.
Size of Logical volume wgroup/wshare changed from 304.00 MiB (38 extents) to 408.00 MiB (51 extents).
Logical volume wgroup/wshare successfully resized.
resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/mapper/wgroup-wshare is mounted on /mnt/wshare; on-line resizing required
old_desc_blocks = 3, new_desc_blocks = 4
The filesystem on /dev/mapper/wgroup-wshare is now 417792 (1k) blocks long.

[root@node2 ~]# df -h
Filesystem                        Size    Used    Avail   Use%    Mounted on
devtmpfs                          867M    0       867M    0%      /dev
tmpfs                             887M    0       887M    0%      /dev/shm
tmpfs                             355M    5.1M    350M    2%      /run
/dev/mapper/rhel_192-root         8.0G    1.6G    6.5G    20%     /
/dev/vda1                         1014M   220M    795M    22%     /boot
tmpfs                             178M    0       178M    0%      /run/user/0
/dev/mapper/wgroup-wshare         378M    2.3M    349M    1%      /mnt/wshare

[root@node2 ~]# lvs
LV      VG      Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
root    rhel_192 -wi-ao---- <8.00g                                                    
swap    rhel_192 -wi-ao---- 1.00g                                                    
wshare  wgroup  -wi-ao----  408.00m  
```

* SUCCESS!!

### Additional Comments: 
IF YOU DO NOT ADD THE -r FLAG, THEN YOU WILL HAVE TO RUN EITHER OF THESE NEXT TWO COMMANDS,
DEPENDING ON IF IT IS XFS OR EXT4:
```
[root@node2 ~]# xfs_growfs /dev/mapper/wgroup-wshare
[root@node2 ~]# resize2fs /dev/mapper/wgroup-wshare
```

