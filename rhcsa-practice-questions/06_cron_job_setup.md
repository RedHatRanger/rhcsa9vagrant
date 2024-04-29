# Add entry to cron

### Question #6:
Set The Cron Job for the user "Natasha" that should runs daily every 1 minute local time and executes "Ex200 Testing" with logger. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### Answer #6:

* 

```
yum install sysstat
systemctl enable sysstat
systemctl start sysstat
```


* As usual it is wise to check what we have already in the system. To see **crontab** we just issue:

```
crontab -l
```

if there are no jobs scheduled then we get information about it.


* edit crontab:

```
crontab -e
# and then editing the file by adding
0 23 * * * /usr/bin/sar -A > /var/log/consumption.log
```

### Additional comment:

The log file does not need to exist - it will be created automatically.
Of course **root** user can edit crontab of every user with flag **-u** specifying user name.
It is worth remembering that there are **/etc/cron.allow** and **/etc/cron.deny**. 
