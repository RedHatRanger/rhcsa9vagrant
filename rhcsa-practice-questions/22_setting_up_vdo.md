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

* After reboot, install ```lvm2```, ```vdo```, and ```kmod-kvdo```.  Then create the volume group: 

```
yum -y install lvm2 vdo kmod-kvdo
```
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/a44b80b3-536e-48ad-a9c7-77ee3cdf33bb)
 
```
systemctl enable --now vdo
```
```
df -h
```
* In this example ```DexTutor``` uses ```/dev/nvme0n3``` as the target device.  You can use whatever comes up that's not mounted in ```df -h```:
```
pvcreate /dev/nvme0n3
```
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/109dacd5-2b49-4a3a-907b-afd042a06f34)


* Next create the volume group ```VG1``` using ```vgcreate```:
```
vgcreate VG1 /dev/nvme0n3
```
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/bbefd14d-b9a4-4b7b-8989-760ef62e4d18)


```
vgs
```
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/2c89323d-1811-463a-a955-4165d54fca0c)

* Run “lvcreate” specifying the type as “vdo” and give it name “VDO1”:
```
lvcreate --type vdo --name VDO1 --size 5GB --virtualsize 50GB VG1
```
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/b35fccec-e011-4f16-90ac-462bce1dc4fe)









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
