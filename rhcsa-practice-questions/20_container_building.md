***On Node2***
# Container Build

### QUESTION #20:
Download containerfile from https://github.com/sachinyadav3496/Text-To-PDF/archive/refs/heads/master.zip 
- Do not make any modification. 
- Build image with this container file.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #20:
* First, you will need to install the Container Management Group packages:
```
[root@node2 ~]# yum install @”Container Management"
```
* Then, switch to the account of user that will be running the container (It is assumed that user named `andrew` exists on the system):

```
[root@node2 ~]# ssh andrew@localhost
[andrew@localhost ~]$ wget https://github.com/sachinyadav3496/Text-To-PDF/archive/refs/heads/master.zip
[andrew@localhost ~]$ unzip master.zip
[andrew@localhost ~]$ rm -rf master.zip
[andrew@localhost ~]$ cd Text-To-PDF-master
[andrew@localhost ~]$ podman build -t myapp .
```
* To check: Type ```podman image ls``` or ```podman images```:
```
[andrew@localhost ~]$ Text-To-PDF-master]$ podman image ls
REPOSITORY                          TAG       IMAGE ID       CREATED         SIZE
localhost/myapp                     Latest    9ba525b6e56a    17 seconds ago  479 MB
registry.fedoraproject.org/fedora   Latest    0e79e93fc530    2 days ago      196 MB
```

* SUCCESS!!
