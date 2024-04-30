***On Node1***

# Configure Chrony for Time Synchronization

### QUESTION #10:
Configure your system to synchronize the time to the Ansible Workstation, ```ansible.mydomain.com```. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #10:
* Install and configure ```chronyd``` on Node1:
```
[root@node1 ~]# yum install chrony
[root@node1 ~]# vim /etc/chrony.conf
```
* Comment out the 'pool' line and add this line below it:
```
#pool 2.rhel.pool.ntp.org iburst
server ansible.mydomain.com iburst maxpoll 16
```
* Now SSH to Ansible or open it up in another tab to open the firewall:
```
[root@ansible ~]# firewall-cmd --permanent --add-service=ntp
success
[root@ansible ~]# firewall-cmd --reload
success
```
* Now, go back to Node1, and restart the Chronyd service and enable it so it starts at boot-time:
```
[root@node1 ~]# systemctl restart chronyd
[root@node1 ~]# systemctl enable chronyd
```
* Finally, to get time synchronization working:
```
[root@node1 ~]# systemctl enable chronyd
```
* You may check your work by running:
```
[root@node1 ~]# chronyc sources -v
```

* SUCCESS!!

### Additional comment:

The old method of time synchronization in RHEL 6 was Ntpd.  This has been completely sunset by Chronyd since RHEL 7.  
