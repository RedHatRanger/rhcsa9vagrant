***On Ansible-NFS Server***

# Make journald persistent between reboots

PURE RHCSA9 QUESTION!

### Question #00:
Configure **journald** to persist between reboots.

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### Answer #00:

* Edit the file /etc/systemd/journald.conf and change "#Storage=auto" to "Storage=persistent":
```
sed -i 's/#Storage.*/Storage=persistent/g' /etc/systemd/journald.conf
systemctl restart systemd-journald.service
```

* SUCCESS!!

### And that's all. After reboot all logs will still be there.

