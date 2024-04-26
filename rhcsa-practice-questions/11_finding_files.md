#  Find files with specific properties

### QUESTION #11 (Part 1):
```
Find All Files in ***/etc*** (not subdirectories) that where modified more than 180 days ago. And copy all of them to a directory /var/tmp/pvt

```

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #11 (Part 1):

* If Your first idea was to use **ls** command - You were wrong. The proper command to actually search for files is (obviously) - **find**.
It is worthwhile to browse through man pages of that command.

* This command provides what is needed
```
find /etc -type f -maxdepth 1 -mtime +180 -exec cp {} /var/tmp/pvt \;
```

### QUESTION #11 (Part 2): 

```
Find all files and directories which is created by a user "natasha" in to this system and copy it into a "/root/natashafiles" directory.
```

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #11 (Part 2):

* Run the find command on the –user and use –exec to copy to the directory:
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/c88333a5-b9f8-414c-860b-231bfa39c283)

* You can alternatively divert the errors in the output using the /dev/null: 
```
find / -user natasha –exec cp –rf {} /root/natashafiles/ \; 2>/dev/null
```

* Alternatively, the test may ask you to put this into a script:
```
vim /opt/filesearch.sh
``` 
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/a9be0bb8-dc84-47f4-b99d-8c89b366ff49)

