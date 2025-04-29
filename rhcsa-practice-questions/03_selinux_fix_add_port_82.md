***On Node1***

# Fix SELinux Port Issues

### QUESTION #3:
FIX SELINUX PORT ISSUES: 
- In your system, httpd service has some files in /var/www/html (do not change or alter files)  
- solve the problem, httpd service of your system having some issues, service is not running on port 82. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #3:

* You first try to troubleshoot the issue by restarting httpd:

```
# THE SETUP:
vim /etc/httpd/conf/httpd.conf
# change the port number from 80 to 82
:wq

[root@node1 ~]# systemctl restart httpd
Job for httpd.service failed because the control process exited with error code.
See "systemctl status httpd.service" and "journalctl -xeu httpd.service" for details.
```

* But as you can see, the server is failing to launch Apache httpd...

```
[root@node1 ~]# systemctl status httpd
● httpd.service - The Apache HTTP Server
   Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; vendor preset: disabled)
   Active: failed (Result: exit-code) since Sun 2023-04-23 08:27:47 IST; 20s ago
     Docs: man:httpd.service(8)
  Process: 31826 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited, status=1/FAILURE)
 Main PID: 31826 (code=exited, status=1/FAILURE)

Apr 23 08:27:47 node1.example.com systemd[1]: Starting The Apache HTTP Server...
Apr 23 08:27:47 node1.example.com httpd[31826]: (13)Permission denied: AH00072: make_sock: could not bind to address [::]:80
Apr 23 08:27:47 node1.example.com httpd[31826]: (13)Permission denied: AH00072: make_sock: could not bind to address 0.0.0.0:80
Apr 23 08:27:47 node1.example.com httpd[31826]: no listening sockets available, shutting down
Apr 23 08:27:47 node1.example.com httpd[31826]: AH00015: Unable to open logs
Apr 23 08:27:47 node1.example.com systemd[1]: httpd.service: Main process exited, code=exited, status=1/FAILURE
Apr 23 08:27:47 node1.example.com systemd[1]: httpd.service: Failed with result 'exit-code'.
Apr 23 08:27:47 node1.example.com systemd[1]: Failed to start The Apache HTTP Server.
```

* This is due to SELINUX File Context issues on port 82...
* Let’s run ```semanage port -l | grep 80``` to see what context port 80 is:

```
[root@node1 ~]# semanage port -l | grep 80
http_cache_port_t              tcp      8080, 8118, 8123, 10001-10010
http_port_t                    tcp      80, 81, 443, 488, 8008, 8009, 8443, 9000
jabber_interserver_port_t      tcp      5269, 5280
```

* As you can see the file context is ```http_port_t``` so lets add port 82 to the list.
* If you are unsure or if you forget you can lookup the syntax in the ```/etc/ssh/sshd_config``` file:
```
[root@node1 ~]# cat /etc/ssh/sshd_config | grep semanage
# semanage port -a -t ssh_port_t -p tcp #PORTNUMBER
root@node1:~# 
```

* As you can see they give you the ```semanage port -a -t http_port_t -p tcp 82``` information in there,
so now we'll just have to apply it:

```
[root@node1 ~]# semanage port -a -t http_port_t -p tcp 82
[root@node1 ~]# semanage port -l | grep 82
amanda_port_t                  tcp      10080-10082
collected_port_t               udp      25826
fac_restore_port_t             tcp      55582
fc_port_t                      tcp      5982
hpip_port_t                    tcp      2208, 2211, 9220, 9221, 9222
hpss_port_t                    tcp      6502, 7002
http_port_t                    tcp      80, 81, 82, 443, 488, 8008, 8009, 8443, 9000
squid_port_t                   tcp      3128, 3401, 4827
trivnet1_port_t                tcp      8200
us_cli_port_t                  tcp      8082, 8083
varnishd_port_t                tcp      6081-6082
```

* Then, we will start and enable httpd.  We can use:
```
[root@node1 ~]# systemctl enable --now httpd
```

* Don’t forget to open the port 82 on the firewall:
```
[root@node1 ~]# firewall-cmd --permanent --add-port=82/tcp
success
[root@node1 ~]# firewall-cmd --reload
success
```

* Now test httpd out:
```
[root@node1 ~]# curl node1.example.com:82
<H1> Welcome to RHCSA Exam !!!! </H1>
```

* SUCCESS!!



### Additional Comments:
BLUF: There is no need to run ```restorecon``` when you invoke ```semanage port```

When you add a port to an SELinux type with semanage port, you're modifying the policy to allow SELinux 
to recognize traffic on that port as a type that has specific rules defined (such as http_port_t for HTTP services). 
This adjustment affects how SELinux manages access control for the port but does not involve file contexts, 
which are what restorecon would modify.

Thus, for the action of adding or modifying port contexts, restorecon is not necessary. The SELinux policy 
update from semanage is sufficient to apply the needed changes immediately, without needing to restore file contexts.

