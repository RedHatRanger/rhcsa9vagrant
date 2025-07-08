# Find Files Using Specific Parameters (Updated on 7/8/2025)

***On Node1***

### QUESTION #11 (Part 1):
Find all the files in the `/etc` directory (not subdirectories) where the files were modified more than 180 days ago, then copy all of them to the `/var/tmp/pvt` directory.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #11 (Part 1):
* The `find` command provides what is needed:
```bash
find /etc -type f -maxdepth 1 -mtime +180 -exec cp {} /var/tmp/pvt \;
```

---

### QUESTION #11 (Part 2): 
Find all files and directories which are owned by the user `natasha` in this system, then copy them into the `/root/natashafiles` directory.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #11 (Part 2):
* Run the `find` command with the `-user` option and use `-exec` to copy to the directory:
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/c88333a5-b9f8-414c-860b-231bfa39c283)

* You can alternatively divert the errors in the output using `/dev/null`: 
```bash
find / -user natasha -exec cp -rf {} /root/natashafiles/ \; 2>/dev/null
```

* Alternatively, the test may ask you to put this into a script:
```bash
vim /opt/filesearch.sh
``` 
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/a9be0bb8-dc84-47f4-b99d-8c89b366ff49)

* Don’t forget to give the `.sh` file execute permissions: 
```bash
chmod 755 /opt/filesearch.sh
```

* **NOTE**: If you want to be able to run the script from any terminal: 
```bash
cp /opt/filesearch.sh /usr/local/bin/
```

### QUESTION #11 (Part 3):
On Node1, find all files in the system that have the setuid bit set and save their paths to `/root/setuid_files.txt`.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #11 (Part 3):
* Use the `find` command with `-perm -4000` to find files with the setuid bit set across the entire system:
```bash
find / -type f -perm -4000 -print > /root/setuid_files.txt 2>/dev/null
```
* **Note**: The `2>/dev/null` redirects permission-denied errors to avoid cluttering the output.

---

### QUESTION #11 (Part 4):
On Node1, find all files in the system that have the setgid bit set and save their paths to `/root/setgid_files.txt`.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #11 (Part 4):
* Use the `find` command with `-perm -2000` to find files with the setgid bit set across the entire system:
```bash
find / -type f -perm -2000 -print > /root/setgid_files.txt 2>/dev/null
```

* SUCCESS!!

---

### Explanation of Parts 3 and 4:

#### Part 3: Finding SUID Files
- **Purpose**: Identifies files with the setuid bit set (e.g., permissions like `-rwsr-xr-x` or 4755), allowing them to execute with the privileges of their owner, often root. Examples include `/usr/bin/passwd` and `/usr/bin/su`.
- **Command Breakdown**:
  - `find /`: Searches the entire filesystem.
  - `-type f`: Limits the search to files only.
  - `-perm -4000`: Matches files with the setuid bit set.
  - `-print`: Prints the full path of matching files (default behavior, included for clarity).
  - `> /root/setuid_files.txt`: Saves the results to a file.
  - `2>/dev/null`: Suppresses permission-denied errors.

#### Part 4: Finding SGID Files
- **Purpose**: Identifies files with the setgid bit set (e.g., permissions like `-rwxr-sr-x` or 2755), allowing them to run with the privileges of the group owner. It’s also used on directories to enforce group ownership inheritance.
- **Command Breakdown**:
  - `find /`: Searches the entire filesystem.
  - `-type f`: Limits the search to files only.
  - `-perm -2000`: Matches files with the setgid bit set.
  - `-print`: Prints the full path of matching files (default behavior, included for clarity).
  - `> /root/setgid_files.txt`: Saves the results to a file.
  - `2>/dev/null`: Suppresses permission-denied errors.
