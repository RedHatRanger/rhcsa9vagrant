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

* Change the partition type to swap, then “w” to write it:

```
Command (m for help): t
Partition number (1,2, default 2): 2
Hex code or alias (type L to list all): 82
Changed type of partition 'Linux' to 'Linux swap / Solaris'.
```

* Don’t forget to run partrobe:
```
[root@node2 ~]# partprobe /dev/vdb
```




* To make swap changes permanent as usual ***/etc/fstab*** must be edited:

```
vi /etc/fstab
/dev/vg/lv_swap2 swap swap defaults 0 0
```

* It is good to check if everything works - the best way is to reboot the system (if possible) and after that issuing:


```
swapon -s
free -k
```

which will show swap/memory statistics.


### Additional comment:

Both commands **mkswap** and **swapon** have parameters like **-L** and **-U** so it is possible to give these commands not only the
path (to the file or partition) but label or UUID.
