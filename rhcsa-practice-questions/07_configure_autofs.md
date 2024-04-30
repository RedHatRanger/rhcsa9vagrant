***On Node1***
# Configure autofs on Node1

### QUESTION #7
Configure autofs to automount the home directories of netuserX user. Note the following: 
- netuser1's home directory is exported via NFS, which is available on node1.mydomain.com and your NFS-exports directory is /netdir for netuser1, 
- netuser1's home directory is node1.mydomain.com:/home/guests/netuserX, 
- netuser1's home directory should be automounted autofs service. 
- home directories must be writable by their users. 
- password for netuser is ```ablerate```. 
 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #7:

* First, you need to install “nfs-utils” and “autofs”:

```
[root@node1 ~]# yum install -y nfs-utils autofs
[root@node1 ~]# vim /etc/auto.master
```
* Add the line ```/netdir /etc/auto.misc``` below the ```/misc``` line: \
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/e0b9c51f-9e0a-483f-b054-12efac7280aa)
```
:wq
```
```
[root@node1 ~]# vim /etc/auto.misc
```
* Insert the line ```netuser1       -fstype=nfs,rw,sync     node1.mydomain.com:/home/guests/netuser``` \
* It should look something like this: \
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/b800a31c-9c7d-4837-b1cf-befe10b2feb5)

* Start and Enable the autofs service:
```
[root@node1 ~]# systemctl enable --now autofs
```

* You may have to open up the nfs ports on the Ansible server:
```
[root@ansible ~]# firewall-cmd --permanent --add-service nfs
[root@ansible ~]# firewall-cmd --permanent --add-service rpc-bind
[root@ansible ~]# firewall-cmd --permanent --add-service mountd
[root@ansible ~]# firewall-cmd --reload
``` 

* Then run the showmount command on Node1:
```
[root@node1 ~]# showmount -e node1.mydomain.com
```
* It should look something like this: \
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/4298cc9c-c7be-49d5-86ab-149c92cf2da2)


SUCCESS!!
   
