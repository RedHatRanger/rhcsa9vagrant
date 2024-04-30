***On Node2***

# Configure Yum Repository

### QUESTION #14:
Configure Your Node2 VM repository installed the packages distribution is available via YUM: \
     - baseos url=http://content.mydomain.com/rocky/x86_64/BaseOS \
     - appstream url=http://content.mydomain.com/rocky/x86_64/AppStream 

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
enabled=1
gpgcheck=0

[AppStream]
name=app
baseurl=http://ansible.mydomain.com/rocky/x86_64/AppStream
enabled=1
gpgcheck=0
```

```
[root@node2 ~]# yum repolist
```
