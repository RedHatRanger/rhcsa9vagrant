***On Node2***

# Configure Yum Repository

### QUESTION #14:
Configure Your Node2 VM repository installed the packages distribution is available via YUM: \
     - baseos url=https://download.rockylinux.org/pub/rocky/8/BaseOS/x86_64/os/ \
     - appstream url=https://download.rockylinux.org/pub/rocky/8/AppStream/x86_64/os/ 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #14:

* First, let's create the repository.  You can copy the text from Node1's /etc/yum.repos.d/pkg.repo:
```
[root@node2 ~]# vim /etc/yum.repos.d/pkg.repo

[BaseOS]
name=base
baseurl=http://ansible.mydomain.com/rocky/x86_64/BaseOS
gpgcheck=0
enabled=1

[AppStream]
name=app
baseurl=http://ansible.mydomain.com/rocky/x86_64/AppStream
gpgcheck=0
enabled=1


:wq
```

```
[root@node2 ~]# yum repolist
```
