# Setup network parameters
### QUESTION #1:
```
Configure TCP/IP and "hostname" as following:  
IP Address: 172.24.40.40/24 
Gateway: 172.24.40.1 
DNS: 172.24.40.1

Hostname: node1.example.com 
```

(scroll down for an answer)
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #1:
* We use the network manager CLI ```nmcli``` or ```nmtui``` to perform this action.

To check which connections are active:
```
nmcli con
```

Option 1: EASY Graphical
```sudo nmtui``` (GUI Configuration)   

Option 2: One-liner command
```nmcli con mod eth1 ipv4.method manual ipv4.addresses 172.24.40.40/24 ipv4.gateway 172.24.40.1 ipv4.dns 172.24.40.1```   

Then:
```
nmcli con down eth1 && nmcli con up eth1
```
Use ```ip addr``` to confirm the IPs are correct.  Then ```ssh root@servera``` 

IF YOU ARE HAVING PROBLEMS WITH SSH: 
```vim /etc/ssh/sshd_config```
Change PermitRootLogin to yes, pubkeyauthentication yes, and PasswordAuthentication yes. 


  
### Additional comment:
It is possible to edit existing connection using ```nmtui``` tool which can be easier. 
Also when using GUI there is also graphical interface for it.
