***On Node1***

# Configure the yum repository for Node1

### QUESTION #2:
Configure your servera VM repository installed the packages distribution is available via YUM: \
    - BaseOS     url = http://ansible.mydomain.com/rocky/x86_64/BaseOS \
    - AppStream  url = http://ansible.mydomain.com/rocky/x86_64/AppStream 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #2:

```
[root@node1 ~]# vim /etc/yum.repos.d/pkgs.repo

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

* To confirm it is configured:
```
[root@node1 ~]# yum clean all
[root@node1 ~]# yum repolist all
```

* Both should be enabled

* SUCCESS!!
