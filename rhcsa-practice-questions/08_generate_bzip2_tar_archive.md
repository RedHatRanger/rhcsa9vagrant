***On Node1***

# Create a tar archive of a file

### QUESTION #8
Create a tar archive of "/etc/" Directory with .bz2 extension. Tar archive named "myetcbackup.tar" should be placed in "/root/" Directory. 
 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #8:

* Use the -j to handle bz2 compression:
```
[root@node1 ~]# tar -cvjf myetcbackup.tar.bz2 /etc/
tar: Removing leading '/' from member names
/etc/
/etc/mtab
/etc/fstab
/etc/crypttab
/etc/lvm/
/etc/lvm/devices/
/etc/lvm/devices/system.devices
/etc/lvm/archive/
/etc/lvm/archive/rhel_192_00000-1332621048.vg
```

* OR if the question asks you to use gzip, use -z instead for compression:
```
[root@node1 ~]# tar -cvzf myetcbackup.tar.gz /etc/
tar: Removing leading '/' from member names
/etc/
/etc/mtab
/etc/fstab
/etc/crypttab
/etc/lvm/
/etc/lvm/devices/
/etc/lvm/devices/system.devices
/etc/lvm/archive/
/etc/lvm/archive/rhel_192_00000-1332621048.vg
```
