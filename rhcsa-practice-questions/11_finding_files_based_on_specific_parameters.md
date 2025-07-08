# Find Files Using Specific Parameters (Updated on 7/7/2025)

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
```bash
find / -user natasha -exec cp -rf {} /root/natashafiles/ \;
```

* You can alternatively divert the errors in the output using `/dev/null`:
```bash
find / -user natasha -exec cp -rf {} /root/natashafiles/ \; 2>/dev/null
```

* Alternatively, the test may ask you to put this into a script:
```bash
vim /opt/filesearch.sh
```
```bash
#!/bin/bash
find / -user natasha -exec cp -rf {} /root/natashafiles/ \; 2>/dev/null
```

* Don’t forget to give the `.sh` file execute permissions:
```bash
chmod 755 /opt/filesearch.sh
```

* **NOTE**: If you want to be able to run the script from any terminal:
```bash
cp /opt/filesearch.sh /usr/local/bin/
```

### QUESTION #11 (Part 3):
On Node1, find all files in the `/usr/bin` directory that have the setuid bit set and save their paths to `/root/setuid_files.txt`.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #11 (Part 3):
* Use the `find` command with `-perm -4000` to find files with the setuid bit set:
```bash
find /usr/bin -type f -perm -4000 > /root/setuid_files.txt
```

* SUCCESS!!
