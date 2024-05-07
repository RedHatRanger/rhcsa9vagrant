***On Node1***

# Configure file access control list (ACL) rights to a file

### QUESTION #9: 

Copy the file /etc/fstab to /var/tmp. Configure the permissions of /var/tmp/fstab so that: \
    - the file /var/tmp/fstab is owned by the root user \
    - the file /var/tmp/fstab belong to the group root \
    - the file /var/tmp/fstab should not be execubable by anyone \
    - the user "natasha" is able to read and write /var/tmp/fstab \
    - the user "harry" can neither write nor read /var/tmp/fstab \
    - all other users (current or future) have the ability to read /var/tmp/fstab

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #9: 

* We start with copying the file with permissions it already has:

```
[root@node1 ~]# cp /etc/fstab /var/tmp
[root@node1 ~]# ls -l /var/tmp/fstab
-rw-r--r--. 1 root root 583 Apr 23 08:52 /var/tmp/fstab
```

* We have to create users mentioned in question (if we do not have them already):

```
useradd natasha
useradd harry
```

* Setting special access rights to files/directories is achieved via **FACL**. The commands are:

```
[root@node1 ~]# setfacl -m u:natasha:rw- /var/tmp/fstab
[root@node1 ~]# setfacl -m u:harry:--- /var/tmp/fstab
```
```
[root@node1 ~]# getfacl /var/tmp/fstab
getfacl: Removing leading '/' from absolute path names
# file: var/tmp/fstab
# owner: root
# group: root
user::rw-
user:harry:---
user:natasha:rw-
group::r--
mask::rw-
other::r--
```
* SUCCESS!!
<br/><br/>
* Additional Comments:
  - By default all other users can read this file so this requirement is already met.
