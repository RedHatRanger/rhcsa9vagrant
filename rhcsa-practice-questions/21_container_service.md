***On Node2***
# Container Service

### QUESTION #21: 
* Configure a container to start automatically 
* Create a container named ```myapp``` using the image which we built in Lab Question #20. 
* Configure the service to automatically mount the directory ```/opt/file``` to container directory ```/opt/incoming```. 
And the user directory ```/opt/processed``` to container directory ```/opt/outgoing``` 
* Configure it to run as a systemd service that should run from the existing user ```andrew``` only
   
* The service should be named ```myapp``` and should automatically start a system reboot without any manual intervention. 

### ANSWER #21:
```
mkdir /opt/file
```
```
mkdir /opt/processed
```
```
chown andrew:andrew /opt/file
```
```
chown andrew:andrew /opt/processed
```
```
ssh andrew@localhost
```
```
podman run -d --name myapp -v /opt/file/:/opt/incoming:Z -v /opt/processed/:/opt/outgoing:Z
```
```
podman ps
```
```
loginctl show-user andrew
```
* Currently, this user cannot add persistent services, so we need to fix that:
```
loginctl enable-linger
```
* Now it will show that it will linger: \
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/08312c87-7e04-4d05-98d0-0badfeef5f63)

* Next, we need to create a folder in the user’s directory called ```~/.config/systemd/user```:
```
mkdir -p ~/.config/systemd/user
```
```
cd ~/.config/systemd/user
```
* We need to generate the service file in that directory: \
```
podman generate systemd --name myapp --files
```
* Next, stop the running container:
```
podman stop myapp
```
* Now start & enable the service:
```
systemctl --user enable --now container-myapp.service
```

SUCCESS!! 
