*** ON NODE1 ***

#  Find files using specific parameters

### QUESTION #11 (Part 1):
Find all the files in the ```/etc``` (not subdirectories) where the files were modified more than 180 days ago, then
copy all of them to the ```/var/tmp/pvt``` directory

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #11 (Part 1):
* The ```find``` command provides what is needed:
```
find /etc -type f -maxdepth 1 -mtime +180 -exec cp {} /var/tmp/pvt \;
```
<br/><br/><br/>
### QUESTION #11 (Part 2): 
Find all files and directories which is created by the user "natasha" in this system, then
copy them into the ```/root/natashafiles``` directory.

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

* Don’t forget to give the .sh file execute permissions: 
```
chmod 755 /opt/file-search.sh
```

NOTE: If you want to be able to run the script from any terminal: 
```
cp /opt/file-search.sh /usr/local/bin/
```

SUCCESS!!
