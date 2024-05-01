***On Node1***

# Add entry to cron

### Question #6 Part 1:
Set The Cron Job for the user "Natasha" that should runs daily every 1 minute local time and executes "Ex200 Testing" with logger. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### Answer #6 Part 1:
* You may reference the syntax setting up cron tasks by running:
```
[root@node1 ~]# cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed
```  

* Run the crontab command for user natasha:
```
[root@node1 ~]# which logger
/bin/logger
[root@node1 ~]# crontab -eu natasha

* * * * * /usr/bin/logger "Ex200 Testing"


:wq
```

* Let's set another cron job an echo "Hello World" message which will go off at 2:30pm every day:
```
[root@node1 ~]# crontab -eu natasha

* * * * * /bin/logger "Ex200 Testing"
30 14 * * * /bin/echo "Hello World"

:wq 
```

* Now let's see what cron jobs are setup under user natasha:
```
[root@node1 ~]# crontab -lu natasha
* * * * *    /usr/bin/logger "Ex200 Testing"
30 14 * * * /bin/echo "Hello World"
```


* SUCCESS!!
<br/><br/><br/><br/>

### Question #6 Part 2:
Configure a task: plan to run echo hello command at 14:23 every day. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### Answer #6 Part 2:
* Run the crontab command: <br/>
```
[root@node1 ~]# crontab -eu natasha
23 14 * * * /bin/echo "hello"
:wq
```
```
[root@node1 ~]# crontab -l natasha
```

* SUCCESS!!



### Additional comment:

The log file does not need to exist - it will be created automatically.
Of course **root** user can edit crontab of every user with flag **-u** **specifying user name**.
It is worth remembering that there are **/etc/cron.allow** and **/etc/cron.deny**. 
