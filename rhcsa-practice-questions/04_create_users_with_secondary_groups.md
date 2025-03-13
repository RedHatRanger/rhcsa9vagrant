## UPDATED ON 2025-03-13:

Official Red Hat Practice Lab [Here](https://zero.apps.open.redhat.com/interactive-labs/zt-rhel.zt-user-basics.prod)

***On Node1***

# Create users and assign them to secondary groups

### QUESTION #4:
Set the default maximum number of days a password may be used to 20 days, then create the following users, groups, and group membership: 
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
vim /etc/login.defs
.
.
.
# Password aging controls:
#
#       PASS_MAX_DAYS   Maximum number of days a password may be used.
#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
#       PASS_MIN_LEN    Minimum acceptable password length.
#       PASS_WARN_AGE   Number of days warning given before a password expires.
#
PASS_MAX_DAYS   20
PASS_MIN_DAYS   0
PASS_WARN_AGE   7


:wq
```
```
[root@node1 ~]# groupadd sysadm
[root@node1 ~]# useradd -G sysadm harry
[root@node1 ~]# useradd -G sysadm natasha
[root@node1 ~]# useradd sarah -s /sbin/nologin

[root@node1 ~]# passwd harry  # provide password for harry
[root@node1 ~]# passwd natasha   # provide password for natasha
[root@node1 ~]# passwd sarah   # provide password for sarah
```

* Be sure to run “passwd” for all users to assign passwords.  You can alternatively run
  these commands in a loop: ```for i in {harry,natasha}; do useradd –G sysadm $i; done```
  and the same for the passwd command. 

* To make sure what groups the users belong to here are the commands:

```
[root@node1 ~]# id {harry,natasha,sarah}
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

* You can set the primary group by using a little "g" instead.
