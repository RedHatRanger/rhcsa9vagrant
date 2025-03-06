***On Node2***
# Reset the root password on Node2 / "Breaking into the System"

### QUESTION #13:
You do not know the root password on ```Node2```, but You have physical access to the machine. Create a new root password and log into the system.
 (It can be preliminary task for starting Your exam. It is crucial to know this procedure by heart.)
***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #13:

# Resetting the Root Password (RHEL 9)

## Objectives
- Reset the root password using this technique: \
![image](https://github.com/user-attachments/assets/9a8d2e87-04ba-48a1-84a7-7fa1d21a3f69)

### Follow these steps:

1. **Reboot the System**

2. **Edit the GRUB Boot Parameters**
   - Use the cursor keys to highlight the first option, then press **e** to edit the `boot parameters`. \
   ![{85C2949A-91EC-444D-BF95-45DABA2BF78E}](https://github.com/user-attachments/assets/be371bea-352a-4f04-ab7d-3614e3f7d835)
   - Now, move the cursor to the 3rd line that starts with `linux`.
   - At the end of this line, add the following parameters:
     ```
     init=/bin/bash

     # Also don't forget to change the 'ro' to 'rw' in the middle of that line.
     ```
   
    ![{E351477F-C3F8-4E05-B1A9-591896C4352F}](https://github.com/user-attachments/assets/46c23b4b-bd24-4dce-b3fc-3fe47c0c0bf4)

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
     echo "root:newpassword" | chpasswd
     ```
   - Follow the prompts to enter and confirm the new password.

6. **Handle SELinux Relabeling**
   - To ensure SELinux contexts are correctly applied, create the `.autorelabel` file:
     ```bash
     touch /.autorelabel
     ```

7. **Force SELinux to Relabel**
   - Force SELinux to relabel during the next boot.
     ```bash
     touch /.autorelabel
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
   - Use the arrow keys to highlight the default boot entry. \
   ![{85C2949A-91EC-444D-BF95-45DABA2BF78E}](https://github.com/user-attachments/assets/be371bea-352a-4f04-ab7d-3614e3f7d835)
   - Press **e** to edit the selected boot entry.
   - Locate the line that starts with `linux` and contains the kernel parameters.
   - At the end of this line, add the following parameters:
     ```
     rw init=/bin/bash
     ```
   ![{D3150F2A-19FF-48B0-85B3-D2DE878B309A}](https://github.com/user-attachments/assets/8665ba1c-ae32-4f62-a1cc-2ce4f92c2839)

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
     echo "root:newpassword" | chpasswd
     ```
   - Follow the prompts to enter and confirm the new password.

6. **Handle SELinux Relabeling**
   - To ensure SELinux contexts are correctly applied, create the `.autorelabel` file:
     ```bash
     touch /.autorelabel
     ```

7. **Reboot the System**
   - Type `exec /sbin/reboot -f` to continue the boot process.
   - The system will reboot and perform an SELinux relabeling, which may take some time.

9. **Verify the New Root Password**
   - After the system reboots, log in as `root` using the new password to confirm the reset was successful.

### Important Considerations
- If your system uses full disk encryption, you will need the decryption passphrase to access the filesystem.
- Always follow your organization's security policies and procedures when performing password resets.

For more detailed information, refer to Red Hat's official documentation on resetting the root password in RHEL 8 and RHEL 9.

SUCCESS!!
