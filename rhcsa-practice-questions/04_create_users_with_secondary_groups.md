# Create users and assign them to secondary groups

### QUESTION #4:
Create the following users, groups, and group membership: 
  - A group named sysadm.
  - A user "harry" who belongs to sysadm as a secondary group.
  - A user "natasha" who belongs to sysadm as a secondary group.
  - A user "sarah" who does not have access to an interactive shell & who is not a member of sysadm group.
  - "harry", "natasha", and "sarah" should all have the password of "password".
  - "sysadm" group has access to add users to the server.
  - "harry" user has access to set password for users without asking sudo password 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #4:

* The answer is pretty straightforward therefore I will provide just the commands: 

```
[root@node1 ~]# groupadd sysadm

[root@node1 ~]# useradd -G sysadm harry
[root@node1 ~]# useradd -G sysadm natasha
[root@node1 ~]# useradd sarah -s /sbin/nologin

[root@node1 ~]# passwd harry  # provide password for harry
[root@node1 ~]# passwd natasha   # provide password for natasha
[root@node1 ~]# passwd sarah   # provide password for sarah
```

![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/db1ef9b2-80da-49c2-8a62-457bec9303bf)

* Be sure to run “passwd” for all users to assign passwords.  You can alternatively run
  these commands in a loop: ```for i in {harry,natasha}; do useradd –G sysadm $i; done```
  and the same for the passwd command. 

* To make sure what groups the users belong to here are the commands:

```
[root@node1 ~]# id harry
[root@node1 ~]# id natasha
[root@node1 ~]# id sarah
```
* You can try to login as **sarah** but it should not work:
```
[root@node1 ~]# su - sarah
```

* Next, we need to search for the binary path of the command useradd so we can update the sudoers file: 
```
[root@node1 ~]# visudo
```
ADD THIS LINE BELOW THE %wheel LINE: <br/>
```
%sysadm ALL=(ALL) /usr/sbin/useradd
```
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/59088040-65da-47b6-9b1b-c8c7cc7cc6e2)

and
```
harry ALL=(ALL) NOPASSWD: /usr/bin/passwd
```
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/4189c358-47f3-4d24-ac75-04f0e9a68d84)

* So Harry can change passwords without sudo privileges (TO BE USED WITH CAUTION!!)

* SUCCESS!!
<br/><br/>

### Note: You can alternatively:
If you just performed ```useradd harry``` and ```useradd natasha``` only, you can modify their memberships:
```
[root@node1 ~]# usermod -aG sysadm harry
[root@node1 ~]# usermod -aG sysadm natasha
```
