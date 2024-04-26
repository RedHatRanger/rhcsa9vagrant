# Restore root password / Breaking into a System

### QUESTION #13:
You do not know the root password on Node2, but You have physical access to the machine. Create a new root password and log into the system.
(It can be preliminary task for starting Your exam. It is crucial to know this procedure by heart.)
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #13:

* During boot time when GRUB loader screen is presented press ```e``` key. That will open an editor with current kernel boot options.
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/088aaa78-f42f-48fe-a477-bbaccb781839)

* Find the line starting with *linux ($root)/vmlinuz...*. Add ```rd.break``` or ```init=/bin/bash``` to the end of that line, and press ```CTRL+x``` simultaneously to restart the 
system with the new option. \
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/12bd0a03-c3a2-4621-a5a6-873cdb740722)

* What this actually does is taking You to the target right at the end of the boot stage - before root filesystem is mounted (on /).
  (SEE THE SCREENSHOT BELOW)
* Type ```mount -o remount,rw /sysroot```. This actually gets You Read-Write access to the filesystem. */sysroot* folder is Your 
normal ***/*** hierarchy.
* Type ```chroot /sysroot``` to make this folder new root directory.
* Now it is time to change the root password (that is what we are here for right?) - type ***passwd*** and provide new
password.
* In order to finish the task **SELinux** must be taken care of. If not, contents of ***/etc/shadow*** will be messed up. There are
two options:

OPTION #1: EASY
```
 /usr/sbin/load_policy -i 
 chcon -t shadow_t /etc/shadow
```

OPTION #2: TAKES LONGER TO ON STARTUP
Create /.autorelabel which will force a relabel on the next boot (and automatically reboot again to apply the fix):
```
touch /.autorelabel
```
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/ffabe4ec-bb00-41ea-b0dc-b5de78e9d56d)


* Type ***exit*** twice (with pressing ENTER after each one)
* Now You can log into the system using the new root password


### Additional comment:

It is possible to edit startup parameters of the kernel from the command line and make it persistent. Just edit **/etc/default/grub**
file and after that make sure to run ```grub2-mkconfig > /boot/grub2/grub.cfg``` in order to apply the changes (for old **BIOS** config). For the **UEFI** computers the final file is **/boot/efi/EFI/redhat/grub.cfg**, or in this case **/boot/efi/EFI/rocky/grub.cfg**.
