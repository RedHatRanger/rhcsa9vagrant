***On Ansible-NFS Server***

# Setting Up Passwordless login from Ansible to Node1 and Node2

### QUESTION #00:
Create SSH keys so that you can connnect to remote servers without a password.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #00:

* Create the user ansible ON ALL 3 systems:
```
[root@ansible ~]# useradd -G wheel ansible; newgrp
[root@node1 ~]# useradd -G wheel ansible; newgrp
[root@node2 ~]# useradd -G wheel ansible; newgrp
```

* Generate the key:
```
[root@ansible ~]# ssh-keygen -t rsa -b 4096 -N ""
[root@ansible ~]# for i in {1..2}; do ssh-copy-id -i ~/.ssh/id_rsa.pub node${i}; done
(Enter the ansible user's password for both nodes)
```

* Then test out login without a password:
```
[root@ansible ~]# ssh ansible@node1
[ansible@node1 ~]$ exit
[root@ansible ~]# ssh ansible@node2
[ansible@node2 ~]$ exit
[root@ansible ~]# 
```

* SUCCESS!!

