## (UPDATE 2025-03-10):
Red Hat now has a [lab](https://www.redhat.com/en/interactive-labs/networking-basics-admin-101) for this to practice for the exam

<br><br>
***On Node1***

# Setup network parameters
### QUESTION #1:
```
Configure TCP/IP and "hostname" as following:  
IP Address: 172.24.40.40/24 
Gateway: 172.24.40.1 
DNS: 172.24.40.1

Hostname: node1.mydomain.com
```

(scroll down for an answer)
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #1:
* We use the network manager CLI ```nmcli``` or ```nmtui``` to perform this action.

* To check which connections are active:
```
[root@node1 ~]# nmcli con
```

Option 1: EASY (GUI Configuration)
```
[root@node1 ~]# nmtui
```

Option 2: One-liner Command:
```
[root@node1 ~]# nmcli con mod eth1 ipv4.method manual ipv4.addresses 172.24.40.40/24 ipv4.gateway 172.24.40.1 ipv4.dns 172.24.40.1
```   

* Then:
```
[root@node1 ~]# nmcli con up eth1
```
* Use ```ip addr``` to confirm the IPs are correct.  Then ```ssh root@172.24.40.40``` 

* Next, to set the hostname:
```
hostnamectl set-hostname node1.mydomain.com
```

* Then type this below to update the new hostname in the terminal:
```
newgrp
```

* SUCCESS!!


  
### Additional comment:
It is possible to edit existing connection using ```nmtui``` tool which can be easier. 
Also when using GUI there is also graphical interface for it.

* NOTE: *IF YOU ARE HAVING PROBLEMS WITH SSH, run*
```
[root@node1 ~]# vim /etc/ssh/sshd_config
```

Change:
  PermitRootLogin to yes
  Pubkeyauthentication yes
  PasswordAuthentication yes. 
