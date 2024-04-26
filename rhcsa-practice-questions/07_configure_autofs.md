***On Node1***
# Configure autofs on Node1

### QUESTION #7
Configure autofs to automount the home directories of netuserX user. Note the following: 
- netuserX home directory is exported via NFS, which is available on classroom.example.com (172.25.254.254) and your NFS-exports directory is /netdir for netuser, 
- netuserX's home directory is classroom.example.com:/home/guests/netuserX, 
- netuserX's home directory should be automounted autofs service. 
- home directories must be writable by their users. 
- password for netuser is ```ablerate```. 
 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #7:

* First, you need to install “nfs-utils” and “autofs”:

```
yum install -y nfs-utils autofs
```
```
vim /etc/auto.master
```
* Add the line ```/netdir /etc/auto.misc``` below the ```/misc``` line:
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/e0b9c51f-9e0a-483f-b054-12efac7280aa)
```
:wq
```
```
vim /etc/auto.misc
```









* Then we have to create config file for **autofs** that holds configuration. 

```
vi /etc/auto.master.d/home.autofs
# and put below content (start point for indirect mounts  -  conf file for this specific mount)
/home/guests /etc/auto.home
```

* As we mention config file in the previous step we have to create it:

```
vi /etc/auto.home
# put below contents there
* -rw,sync classroom.example.com:/home/guests/&
```

using wildcard * sign as a first parameter and & as the same in path on remote server will make sure that any user that is logged 
using LDAP will have its name used as parameter for mounting.

* Make sure that the service **autofs** is enabled and started:

```
systemctl enable autofs.service
systemctl start autofs.service
```

* Just SSH locally with user that is supposed to exist on LDAP server and check the home directory:

```
ssh ldapuser5@localhost
cd
pwd  # it should be /home/guests/ldapuser5
```


### Additional comment:

**autofs** is working in user space - and mounting network shares via **/etc/fstab** is using root context

Name of the config file in **/etc/auto.master.d** is not important - the only important thing is that is must have **.autofs** suffix
