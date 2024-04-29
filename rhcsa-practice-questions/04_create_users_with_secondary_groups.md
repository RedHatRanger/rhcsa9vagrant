# Create users and assign them to secondary groups

### QUESTION #4:
Create the following users, groups, and group membership: 
  - A group named sysadm.
  - A user "harry" who belongs to sysadm as a secondary group.
  - A user "natasha" who belongs to sysadm as a secondary group.
  - A user "sarah" who does not have access to an interactive shell & who is not a member of sysadm group.
  - "harry", "natasha", and "sarah" should all have the password of password.
  - "sysadm" group has access to add users to the server.
  - "harry" user has access to set password for users without asking sudo password 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #4:

* The answer is pretty straightforward therefore I will provide just the commands: 

```
groupadd sysadm

useradd -G sysadm harry
useradd -G sysadm natasha
useradd sarah -s /sbin/nologin
OR
usermod -aG sysadm harry
usermod -aG sysadm natasha


for i in {harry, natasha, sarah}; do passwd $i; done
OR
passwd harry  # provide password for andrew
passwd natasha   # provide password for susan
passwd sarah   # provide password for sarah
```

![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/db1ef9b2-80da-49c2-8a62-457bec9303bf)


* To make sure what groups the users belong to here are the commands:

```
id harry
id natasha
id sarah

* You can try to login as **sarah** but it should not work:

```
su - sarah
```
