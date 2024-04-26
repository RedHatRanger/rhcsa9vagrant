# Red Hat Enterprise Linux RHCSA 9 Vagrant Lab for Exam EX200
# Updated on 2024-04-26
# RHCSA 9 Lab - Vagrant Practice Lab using Rocky 9.3

# How to use this lab
- install virtualbox
- install vagrant
- Open a PowerShell Terminal Window
  ```
  mkdir ~/vagrant
  ```
  ```
  cd ~/vagrant
  ```
- Download this repo zip file and place it in the "vagrant" directory
- Unzip the contents and cut and paste them to the "vagrant directory
- Delete the .zip file and the empty folder you extracted everything from
- Run ```vagrant up``` inside this directory and it will automatically build off of the Vagrantfile:
    
The vagrant script will set up 3 Rocky 9 VMs: ansible, node1, and node2. 

In three separate terminals, you want to execute the following commands to ssh into the boxes:

```
vagrant ssh ansible
```

```
vagrant ssh node1
```

```
vagrant ssh node2
```

From there, you can change the settings in /etc/ssh/sshd_config, change root password, and so forth.

TO STOP ALL RUNNING VMs, RUN
```
vagrant halt
```
TO REBOOT ALL THE VMs, RUN
```
vagrant reload
```
TO DELETE (BLOW AWAY) ALL VMs, RUN
```
vagrant destroy
```

My "To do" list:
- This has only been tested on Windows 11. This should also work fine with Linux distros as well...but you will need to use the commands in the Vagrantfile for libvirt instead...I will soon be uploading that version, so stay tuned!


Special Thanks to: https://github.com/gitmpr for providing the original Vagrantfile.  I've optimized it to most up-to-date version of Rocky 9 for use with Virtualbox and Windows 10/11.

Please refer to the official Vagrant documentation here: https://developer.hashicorp.com/vagrant/tutorials/getting-started/getting-started-boxes
