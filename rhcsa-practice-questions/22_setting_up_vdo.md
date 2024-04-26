# Configure block device to use VDO

### QUESTION #22 - VDO
Create a VDO named vdo1 of size 50Gb and mount it at /vdo_m. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #22:

* **VDO** is a way to provide deduplication and compression of data on the disc. Worth read material can be found
<a href="https://hobo.house/2018/09/13/using-vdo-on-centos-rhel7-for-storage-efficiency/">here</a>.

* First, attach storage to your physical or virtual machine. Then, run:
```
lsblk
```
```
yum -y update
```
```
init 6
```

* Install ```lvm2```, ```vdo```, and ```kmod-kvdo```.  Then create the volume group: 

```
yum -y install lvm2 vdo kmod-kvdo
``` 
```
systemctl enable --now vdo
```



* Creating VDO is easy:

```
vdo create --name=SOME_NAME --device=/dev/BLOCK_DEVICE --vdoLogicalSize=SIZE
```

* size should be different depending on the type of objects stored. For containers the multiplier of size can be up to
**10 times** than size of block device. For object storage it can be up to **3 times**. So eg. for **20GB** device we want to 
use for object storage we can set the size of VDO to **60GB**. 

* when creating VDO we do not want to discard blocks when making the filesystem. So use proper switch (see man pages) for specific 
type of filesystem being created.

```
mkfs.xfs -K /dev/LINK
# or
mkfs.ext4 -E nodiscard /dev/LINK
```

after this make sure to refresh registers:

```
udevadm settle
```

* mounting of VDO is simple but requires adding additional properties to ***/etc/fstab*** file when doing it:

```
UUID=smething  /mount_point   FS_TYPE  _netdev,x-systemd.device-timeout=0,x-systemd.requires=vdo.service 0 0 
```

### Additional comment:

We require over **500MB** of RAM for every TB of storage to apply VDO.

The commands to check status and size of VDOs are:  **vdo** and **vdostats**
