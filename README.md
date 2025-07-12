# Red Hat Enterprise Linux RHCSA 9 Vagrant Lab for Exam EX200 using Rocky 9.6

# Updated on 2025-07-11

# How to use this lab
- [Install virtualbox](https://download.virtualbox.org/virtualbox/7.1.6/VirtualBox-7.1.6-167084-Win.exe)
- [Install vagrant](https://releases.hashicorp.com/vagrant/2.4.3/vagrant_2.4.3_windows_amd64.msi)
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
```
vagrant up
```
    
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

My "To do" List:
- This has only been tested on Windows 11. This should also work fine with Linux distros as well...but you will need to use the commands in the Vagrantfile for libvirt instead...I will soon be uploading that version, so stay tuned!
- Investigate using this to get GUI working on the vagrant boxes: \
  config.vm.provision "shell", inline: <<-SHELL \
 sudo yum -y groupinstall @"GNOME Desktop" \
 sudo systemctl set-default graphical.target \
 sudo systemctl isolate graphical.target \
SHELL

investigate this lab for ssh:
https://youtu.be/WDDkDw3LI3U?si=0B42I4X_EpYB-t-q

and this lab for journald persistence:
https://youtu.be/gbzAVB8ie3Y?si=H_k48QkYJZGAbSnm

Create a lab for ssh-keygen

Special Thanks to: https://github.com/gitmpr for providing the original Vagrantfile.  I've optimized it to most up-to-date version of Rocky 9 for use with Virtualbox and Windows 11.

Please refer to the official Vagrant documentation here: https://developer.hashicorp.com/vagrant/tutorials/getting-started/getting-started-boxes
