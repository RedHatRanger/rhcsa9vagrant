# Add entry to cron

### Question #6 Part 1:
Set The Cron Job for the user "Natasha" that should runs daily every 1 minute local time and executes "Ex200 Testing" with logger. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### Answer #6 Part 1:
* Run the crontab command: <br/>
```
crontab -e -u natasha
```

![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/e47480e7-66a3-4785-b2f5-71585acac866)

* As usual it is wise to check what we have already in the system. To see **crontab** we just issue:

```
crontab -l
```

* if there are no jobs scheduled then we get information about it.

* SUCCESS!!


### Question #6 Part 2:
Configure a task: plan to run echo hello command at 14:23 every day. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### Answer #6 Part 1:
* Run the crontab command: <br/>
```
crontab -e -u natasha
```
```
23 14 * * * /bin/echo "hello"
```
```
crontab -l natasha
```

* SUCCESS!!

### Additional comment:

The log file does not need to exist - it will be created automatically.
Of course **root** user can edit crontab of every user with flag **-u** **specifying user name**.
It is worth remembering that there are **/etc/cron.allow** and **/etc/cron.deny**. 
