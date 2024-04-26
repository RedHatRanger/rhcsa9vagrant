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
chown andrew:andrew /opt/processed
```
