***On Node1***

# Configure the yum repository for Node1

### QUESTION #2:
Configure Your Node1 VM repository installed the packages distribution is available via YUM: \
    - baseos url=https://download.rockylinux.org/pub/rocky/8/BaseOS/x86_64/os/ \
    - appstream url=https://download.rockylinux.org/pub/rocky/8/AppStream/x86_64/os/

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #2:

```
[root@node1 ~]# vim /etc/yum.repos.d/pkgs.repo

[BaseOS]
name=base
baseurl=https://download.rockylinux.org/pub/rocky/8/BaseOS/x86_64/os/
gpgcheck=1
enabled=1

[AppStream]
name=app
baseurl=https://download.rockylinux.org/pub/rocky/8/AppStream/x86_64/os/
gpgcheck=1
enabled=1


:wq
```

* To confirm it is configured:
```
[root@node1 ~]# yum clean all
[root@node1 ~]# yum repolist all
```

* Both should be enabled

* SUCCESS!!
