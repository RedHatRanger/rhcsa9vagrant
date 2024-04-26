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
* First, you will need to run
```
yum install @”Container Management"
```
* Then, switch to the account of user that will be running the container. It is assumed that user named `andrew` exists on the system \
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/3c976edc-3aec-4a0a-b247-ccfec11b62b7)

```
ssh andrew@localhost
```
```
wget https://github.com/sachinyadav3496/Text-To-PDF/archive/refs/heads/master.zip
```
```
unzip master.zip
```
```
rm -rf master.zip
```
```
cd Text-To-PDF-master
``` 
```
podman build -t myapp .
```
* To check: Type ```podman image ls``` or ```podman images```: \
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/5210ba14-17a5-4f4f-bc0d-00d3efe570e4)

SUCCESS!!