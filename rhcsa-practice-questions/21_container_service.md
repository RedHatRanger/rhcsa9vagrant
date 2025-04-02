## UPDATE 2025-03-10:
- Red Hat now provides a [FREE LAB](https://www.redhat.com/en/configure-a-rootless-podman-service) for this task

<br><br>
***On Node2***
# Container Service

### QUESTION #21: 
* Configure a container to start automatically 
* Use the container named ```myapp``` which we built in Lab Question #20. 
* Configure the service to automatically mount the directory ```/opt/files``` to container directory ```/opt/incoming```. 
And the user directory ```/opt/processed``` to container directory ```/opt/outgoing``` 
* Configure it to run as a systemd service that should run from the existing user ```andrew``` only
* The service should be named ```myapp``` and should automatically start a system reboot without any manual intervention. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #21:
```
[root@node2 ~]# mkdir /opt/files /opt/processed
[root@node2 ~]# chown andrew:andrew /opt/files /opt/processed
[root@node2 ~]# loginctl enable-linger andrew
[root@node2 ~]# ssh andrew@localhost
[andrew@node2 ~]$ podman run -d --name myapp -v /opt/files/:/opt/incoming:Z -v /opt/processed/:/opt/outgoing:Z myapp
[andrew@node2 ~]$ podman ps

CONTAINER ID   IMAGE                           COMMAND                  CREATED         STATUS          PORTS                 NAMES
af03e63960ad   localhost/myapp                 /usr/bin/run-http...    3 seconds ago   Up 3 seconds                           myapp
```

* Let's see if Andrew has the right permissions to have Linger:

```
[andrew@node2 ~]$ loginctl show-user andrew
ID=1002
...
Timestamp=Sun 2023-04-23 09:54:45 IST
TimestampMonotonic=2614502454
RuntimePath=/run/user/1002
Service=user@1002.service
Slice=user-1002.slice
Display=5
State=active
Sessions=5
IdleHint=no
IdleSinceHint=1682224288866742
IdleSinceHintMonotonic=3017922428
Linger=yes
```

* Next, we need to create a folder in the user’s directory called ```~/.config/systemd/user```:
```
[andrew@node2 ~]$ mkdir -p ~/.config/systemd/user
[andrew@node2 ~]$ cd ~/.config/systemd/user
```

* We need to generate the service file in that directory: 
```
[andrew@node2 user]$ podman generate systemd --name myapp --new --files
/home/andrew/.config/systemd/user/container-myapp.service
[andrew@node2 user]$ systemctl --user daemon-reload
```

* Next, stop the running container:
```
[andrew@node2 user]$ podman stop myapp
```

* Now start & enable the service:
```
[andrew@localhost ~]$ systemctl --user enable --now container-myapp.service
```

* SUCCESS!!

### Additional Information:

Syntax: 
```
$ podman run -v <src>:<dst>:Z <image> 

# Add port to the firewall 
firewall-cmd --add-port=8080/tcp --permanent 
firewall-cmd --reload 
```

### BONUS: Alternate lab from Red Hat:
```
yum install -y podman
mkdir -p ~/.config/containers/systemd/

cat << EOF > ~/.config/containers/systemd/httpd.container
[Service]
Restart=always

[Container]
ContainerName=httpd
Image=docker.io/library/httpd
Label="io.containers.autoupdate=registry"
Environment=TZ=America/Vancouver
Environment=VERSION=docker
Volume=httpd-data.volume:/usr/local/apache2/htdocs:Z
PublishPort=8080:80/tcp

[Install]
WantedBy=default.target
EOF

cat << EOF > ~/.config/containers/systemd/httpd-data.volume
[Volume]
User=garfield
Group=garfield
EOF

systemctl --user daemon-reload

systemctl --user start httpd.service

podman ps

systemctl --user status httpd --no-pager

systemctl --user stop httpd

curl http://localhost:8080
```

