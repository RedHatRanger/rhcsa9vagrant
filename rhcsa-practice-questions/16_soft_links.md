***On Node2***
# Create a Soft Link

### QUESTION #16:
Create a soft link for /var/log/messages in the /root directory.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #16:

* This is pretty straight forward, but you need to know that the source file you are referencing comes first,
then the location you want to put it.
* ***THE SYNTAX IS*** ```ln -s <src> <dst>```

```
[root@node2 ~]# ln -s /var/log/messages /root
[root@node2 ~]# ls -l /root
-rw-r--r--. 1 root root  591 Nov  2  2022 google-cloud.repo
lrwxrwxrwx. 1 root root   17 May  2 19:06 messages -> /var/log/messages
-rw-r--r--. 1 root root   54 May  2 19:04 post-run.log
-rw-r--r--. 1 root root 5121 Jun  3  2022 rh-cloud.repo
-rw-r--r--. 1 root root   43 May  2 19:03 webconsoleurl.txt

[root@node2 ~]# tail -n 5 /root/messages
May  2 19:05:54 rhel-9-1-6-20-2023 systemd[1]: cockpit-wsinstance-http.socket: Deactivated successfully.
May  2 19:05:54 rhel-9-1-6-20-2023 systemd[1]: Closed Socket for Cockpit Web Service http instance.
May  2 19:05:54 rhel-9-1-6-20-2023 systemd[1]: cockpit-wsinstance-https-factory.socket: Deactivated successfully.
May  2 19:05:54 rhel-9-1-6-20-2023 systemd[1]: Closed Socket for Cockpit Web Service https instance factory.
May  2 19:06:10 rhel-9-1-6-20-2023 systemd[1]: systemd-hostnamed.service: Deactivated successfully.
total 20
```

* Now, let's check the inode number on the far left of the output.  They should be different:
```
[root@node2 ~]# ls -li /var/log/messages
5850 -rw-------. 1 root root 130632 May  2 19:13 /var/log/messages
root@node2:~# ls -li /root/messages
295363 lrwxrwxrwx. 1 root root 17 May  2 19:06 /root/messages -> /var/log/messages
```

* SUCCESS!!
