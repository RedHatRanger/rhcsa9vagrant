***On Node1***

# Create a shared directory by group

### QUESTION #5: 

Create a collaborative directory /shared/sysadm with the following characteristics: \
    - Group ownership of /shared/sysadm is sysadm. \
    - The directory should be readable, writable, and accessible to member of sysadm, but not to any other user. \
    - (It is understood that root has access to all files and directories on the system.) \
    - Files created in /shared/sysadm automatically have group ownership set to the sysadm group. \
    - Finally, we will add two users to the group to give them access to the shared folder.
    
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #5: 

* As mentioned above - creation of user was covered in Lab ***04_create_users_with_secondary_groups***.

* First we create a directory which is pretty straightforward (however creating user-related folders in root hierarchy is not a good idea)

```
[root@node1 ~]# mkdir -p /shared/sysadm
```

* Then we create the group and assign it as group owner of the created folder:

```
[root@node1 ~]# groupadd sysadm
[root@node1 ~]# chgrp sysadm /shared/sysadm
```


* Here is the most tricky part. To achieve our goal we will be using SETGUID or **SGID**, which is a concept that users operating on the files are
given the same permissions like to group owner of the folder/file. We've already set group owner for the folder so now we assign permissions to it.   
```
[root@node1 ~]# chmod 2770 /shared/sysadm
```

* Notice the ***2*** prefix in the access rights. This is a **Special Permission Bit** that sets up **SGID**. 
* That means that any user from the group that owns the file/folder will be given group permissions when operating on it. 
* It simplifies administration as You do not have to give users access via **ACLs** to the folder, rather than just assign them to the group and that's it.
<br/><br/>
* Finally we need to remember to actually add our users to the mentioned group **sysadm**.
  
```
[root@node1 ~]# usermod -aG sysadm harry
[root@node1 ~]# usermod -aG sysadm natasha
```

* SUCCESS!!

### Additional comment:

* It is possible to change user's primary group for the current session using **newgrp** command.


