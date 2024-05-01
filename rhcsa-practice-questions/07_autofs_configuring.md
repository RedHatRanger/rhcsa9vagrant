***On Node1***
# Configure autofs on Node1

### QUESTION #7
Configure autofs to automount the home directories of remoteuserx user. Note the following: 
   - remoteuserx directory is exported via NFS, which is available on classroom.example.com (192.168.13.162) and your NFS-exports directory is /ourhome/remoteuserx for remoteuserx, 
   - remoteuserx's home directory should be automounted using autofs service. 
   - home directories must be writable by their users. 
 
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #7:

* First, you need to install ```autofs```:

```
[root@node1 ~]# yum install -y autofs
[root@node1 ~]# systemctl enable --now autofs
[root@node1 ~]# vim /etc/auto.master.d/aa.autofs
/-      /etc/auto.misc


:wq
```

* Next edit the /etc/auto.misc file:
```
[root@node1 ~]# vim /etc/auto.misc
```
* Insert the line ```netuser1       -fstype=nfs,rw,sync     node1.mydomain.com:/home/guests/netuser``` \
* It should look something like this: \
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/b800a31c-9c7d-4837-b1cf-befe10b2feb5)

* Start and Enable the autofs service:
```
[root@node1 ~]# systemctl restart autofs
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
   
