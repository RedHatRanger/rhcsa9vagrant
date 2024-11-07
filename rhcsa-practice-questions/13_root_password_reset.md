![1000008791](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/f5967b24-cef6-458f-8d5d-4f505879aca5)
***On Node2***
# Restore root password on Node2 / Breaking into the System

### QUESTION #13:
You do not know the root password on ```Node2```, but You have physical access to the machine. Create a new root password and log into the system.
 (It can be preliminary task for starting Your exam. It is crucial to know this procedure by heart.)
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #13:

# Resetting the Root Password (RHEL 8 and RHEL 9)

## Objectives
- Reset the root password.

### Resetting the Root Password (RHEL 8):
When the root password for a system is lost or forgotten, an authorized administrator can reset it. Some methods can work remotely, such as through an SSH connection, while others require physical console access.

If any user is still logged in as the root account on an unlocked terminal, then use that active session to change the password. Similarly, use any accessible account that has sufficient sudo shell or passwd command access to reset the root password.

A more complex method is to manually edit the `/etc/shadow` file by copying in a known password hash from any account that has sudo text editor access, or by editing the virtual machine's disk image with the `guestfish` command.

#### Rescue Mode
If the previous methods are not available or unsuccessful, then rescue mode is an alternative. An administrator with physical system access can use the anaconda installation program's rescue mode to boot from installation media or an external media device, and then use that access to change the root password. This procedure requires that either the system's disks are not encrypted or that the encryption password is known. If the firmware password is configured and known, then you can boot from alternative devices on both BIOS and UEFI firmware. The anaconda rescue mode is accessed by booting from a Red Hat Enterprise Linux 8 boot media device, such as a USB installation device, and selecting the Troubleshooting option in the boot menu.

#### Resetting the Root Password without External Media
When the external boot media method is not available or unsuccessful, an administrator can use the systemd startup sequence to halt the initial ramdisk (initramfs) startup sequence. This method requires physical console access, or access through a remote management card or KVM switch, and knowledge of passwords for disk encryption and the boot loader, if configured.

This method for resetting a root password consists of these steps:

1. **Reboot the System**
   - Reboot the system and interrupt the boot loader timer by pressing any key except **Enter**.

2. **Edit the GRUB Boot Parameters**
   - Find the entry that is normally booted, and change it so that it halts execution during the initial ramdisk startup sequence.
   - Use the cursor keys to highlight the entry that would normally be booted, and press **e**.
   - Use the cursor keys to move to the line that has the kernel and the kernel arguments. This line normally starts with `linux`.
   - At the end of this line, add the following parameters:
     ```
     rw.break
     ```
     
   - Press **Ctrl+x** to boot with the modified parameters.

3. **Access the Root Shell**
   - The system now boots, but exits the process during initial ramdisk execution. If a prompt does not appear shortly, press **Enter** to see whether the prompt is obscured by kernel output.

4. **Remount the Root Filesystem**
   - Remount the root file system with read and write capabilities. The file system is currently mounted on the `/sysroot` directory mount point.
     ```bash
     switch_root:/# mount -o remount,rw /sysroot
     ```

5. **Change the Working Root Directory**
   - Change the working root directory to `/sysroot`.
     ```bash
     switch_root:/# chroot /sysroot
     ```

6. **Reset the Root Password**
   - Reset the root password to a known value.
     ```bash
     sh-4.2# echo "root:newpassword" | chpasswd
     ```

7. **Force SELinux to Relabel**
   - Force SELinux to relabel during the next boot.
     ```bash
     sh-4.2# touch /.autorelabel
     ```

   **Important:** The SELinux relabel in this method is required. SELinux detects whether an alternative access sequence occurred because the SELinux contexts are no longer present on the modified files. To trust the system again, SELinux will not boot until all files are properly relabeled.

8. **Reboot the System**
   - Reboot the system by exiting from the chroot environment and from the switch_root prompt by typing `exit` twice.

9. **Verify the Root Password**
   - Verify that the root password access is reset by either logging in as `root` or by logging in as a non-privileged user and switching to root with any method that requires entering the root password.

<br/><br/><br/><br/><br/><br/>

### Resetting the Root Password (RHEL 9):
In Red Hat Enterprise Linux (RHEL) 9, the process for resetting the root password has been updated to enhance system security. The traditional method using `rd.break` may not function as expected in this version. Instead, you can reset the root password by modifying the kernel parameters during the boot process. Here's how:

1. **Reboot the System**
   - Restart your system.
   - During the boot process, when the GRUB menu appears, press any key (except **Enter**) to interrupt the automatic boot sequence.

2. **Edit the GRUB Boot Parameters**
   - Use the arrow keys to highlight the default boot entry.
   - Press **e** to edit the selected boot entry.
   - Locate the line that starts with `linux` and contains the kernel parameters.
   - At the end of this line, add the following parameters:
     ```
     rw init=/bin/bash
     ```
   - Press **Ctrl+x** to boot with the modified parameters.

3. **Access the Root Shell**
   - The system will boot into a single-user mode with a root shell prompt.

4. **Remount the Root Filesystem**
   - By default, the root filesystem is mounted as read-only. Remount it with read and write permissions:
     ```bash
     mount -o remount,rw /
     ```

5. **Reset the Root Password**
   - Set a new root password by executing:
     ```bash
     passwd
     ```
   - Follow the prompts to enter and confirm the new password.

6. **Handle SELinux Relabeling**
   - To ensure SELinux contexts are correctly applied, create the `.autorelabel` file:
     ```bash
     touch /.autorelabel
     ```

7. **Reboot the System**
   - Type `exec /sbin/init` to continue the boot process.
   - The system will reboot and perform an SELinux relabeling, which may take some time.

8. **Verify the New Root Password**
   - After the system reboots, log in as `root` using the new password to confirm the reset was successful.

### Important Considerations
- If your system uses full disk encryption, you will need the decryption passphrase to access the filesystem.
- Always follow your organization's security policies and procedures when performing password resets.

For more detailed information, refer to Red Hat's official documentation on resetting the root password in RHEL 8 and RHEL 9.

SUCCESS!!
