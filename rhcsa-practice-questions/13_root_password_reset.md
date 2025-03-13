## UPDATED ON 2025-03-13:

Tutorial Video Link [Here](https://www.youtube.com/watch?v=-ZH4M-VShN8)

<br><br>
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

## Instructions:

<br>
1. Reboot the System

<br><br>
2. **Edit the GRUB Boot Parameters**
   - Use the cursor keys to highlight the first option, then press `e` to edit the `boot parameters`. \
   ![{85C2949A-91EC-444D-BF95-45DABA2BF78E}](https://github.com/user-attachments/assets/be371bea-352a-4f04-ab7d-3614e3f7d835)
   - Now, move the cursor to the 3rd line that starts with `linux`.
   - At the end of this line, add the following parameters:
     ```
     init=/bin/bash

     # Also don't forget to change the 'ro' to 'rw' in the middle of that line.  See example screen below
     ```

   ![image](https://github.com/user-attachments/assets/351f17d2-aa52-4f1e-a327-6d08382c197a)

   - Press **Ctrl+x** to boot with the modified parameters.

<br><br>
3. **Access the Root Shell**
   - The system will boot into a single-user mode with a root shell prompt.

<br><br>
4. **Reset the Root Password**
   - Set a new root password by executing:
     ```bash
     passwd
     ```
   - Follow the prompts to enter and confirm the new password.

<br><br>
5. **Force SELinux to Relabel**
   - Force SELinux to relabel during the next boot.
     ```bash
     touch /.autorelabel
     ```

   **Important:** The SELinux relabel in this method is required. SELinux detects whether an alternative access sequence occurred because the SELinux contexts are no longer present on the modified files. To trust the system again, SELinux will not boot until all files are properly relabeled.

<br><br>
6. **Reboot the System**
```
exec /sbin/init
```

<br><br>
7. **Verify the Root Password**
   - Verify that the root password access is reset by either logging in as `root` or by logging in as a non-privileged user and switching to root with any method that requires entering the root password.

<br><br>
* SUCCESS!!
