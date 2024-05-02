Before starting anything, build an nfs server.

	yum install rpcbind nfs-utils

	vim /etc/exports
	
        	 /share        192.168.0.0/24(rw,no_root_squash)

	systemctl enable --now rpcbind

	systemctl enable --now nfs-server

	firewall-cmd --add-service [ mountd rpc-bind nfs-server ]

------------------------------------------------------------------------------------------------------------


root password

     During boot add "init=/bin/bash rw" to the end of the linux grub line

     grep root /etc/shadow

     ls -Z /etc/shadow

     passwd

     grep root /etc/shadow

     chcon <SeeEarlier -Z> /etc/shadow

     ls -Z /etc/shadow

     restart system

------------------------------------------------------------------------------------------------------
SSH
	ssh-keygen
	
	ssh-copy-id <hostname>

------------------------------------------------------------------------------------------------------
tar examples

    tar -czf mytar.tar.gz     /dir/to/tarup    #   -z is for gzip
   
    tar -cjf  mytar.tar.bz2   /dir/to/tarup    #    -j is for bzip2

    tar -zxv my.tar.gz	    		       #    creates ./dir/to/tarup

    tar -cjf my.tar.bz --directory=/dir/to/ tarup	#starts the archive at the tarup level versus the dir level


------------------------------------------------------------------------------------------------------
hard and soft links
	ln file1 file2	#creates a hard link - where both file1 and file2 point to the same disk block
			#can only be used on files... not directories
			#cannot cross luns. 
			#Edit one file and you change the other
			#even chmod one file changes the other
			#the number in column 2 tels you the number of hard links associates with file

	ls -s file1 file2	#only creates a pointer to file1. 
				#rm file 1 and the link is dead. 
				#can cross luns

	find / -xdev -samefile /root/file/whatever

------------------------------------------------------------------------------------------------------
Create simple shell scripts

	#!/bin/bash
	
	echo the first input is $1, the second input is $2
	if [ $1 -lt 10 ]; then 
		echo "input 1 ($1) is less than 10"
	else 
		echo "input 1 ($1) is more than 10"
	fi
	
	#What happens when you change the double quotes to single quotes?	



------------------------------------------------------------------------------------------------------
Identify CPU/memory intensive processes and kill processes

	#method 1
	top		#to identify processes percentages
			#inside top type k, give it the pid, then change 15 to 9

	#method 2
	ps aux|less	# scroll to find the process you want to kill
	kill -9 <pid>

------------------------------------------------------------------------------------------------------


Niceness

     -20 most favorable to process, 19 least favorable

     nice -n -5 <program name>   # to set nice when starting a program

     use htop or ps to identify the pid of your process

     renice  -n -5 -p 1234    # to change the nice value of an already running pid




------------------------------------------------------------------------------------------------------

Process scheduling

     chrt -m     # to show the possible priorities with min/max prioritiesman

     chrt -p <pid>    #show pids current priority

     chrt -f -p 50 <pid>    #change pid to fifo and priority 50

     chrt -p <pid>

     chrt -f -p 50 </command>    # to start a command with fifo priority 50

------------------------------------------------------------------------------------------------------

Manage tuning profiles

     dnf install tuned

     systemctl enable --now tuned

     tuned-adm -h

     tuned-admin

     dnf list all | grep tuned-profiles

     dnf install <tuned-profile-xxx>

     dnf profile desktop

     systemctl restart tuned


------------------------------------------------------------------------------------------------------
How to find and interpret system log files on Linux

     /var/log/messages  # of course

     journalctl -n 100    # show the last 100 journal messages

     jounralctl -f    # like tail -f

     journalctl -h

     journalctl -u sshd

     jurnalctl _UID=1234    #this isn't in the man page

------------------------------------------------------------------------------------------------------
Preserve System Journals

https://usg01.safelinks.protection.office365.us/?url=https%3A%2F%2Fwww.redhat.com%2Fsysadmin%2Frsyslog-systemd-journald-linux-logs&data=05%7C02%7Ckyle.j.lorensen.ctr%40army.mil%7Cdedb3697f3784fcb40c308dc1bc29288%7Cfae6d70f954b481192b60530d6f84c43%7C0%7C0%7C638415769722732484%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000%7C%7C%7C&sdata=ID3JN%2FKwIeVERBNhg6FbxCXYAn5JTikmU8k3YGtnaFU%3D&reserved=0

     mkdir /var/log/journal

     vim /etc/systemd/journald.conf

         Storage=persistent

     systemctl restart systemd-journald

     journalctl --flush

------------------------------------------------------------------------------------------------------


emergency.targetMSDOS and GPT partitition tables

     MBR(msdos)

        Only supports parts up to 2T

         Supports 3 primary and 1 extended part or 4 primary

         In an extended partition, make logical partitions

     GPT

         Use this whenever possible




--------------------------------------------------------------------------------------------------------
Messing with swap

     grep swap /etc/fstab

     swapoff -a

     #make a disk partition for the swap size you want, lvm or physical doesn't matter

     mkswap /dev/myvg/mylv

     vim /etc/fstab

         /dev/myvg/mylv    none    swap    defaults    0 0

     swapon -a

     free -h

     cat /proc/swaps

     top

------------------------------------------------------------------------------------------------------------

Create, mount, unmount, and use vfat, ext4, and xfs file systems
   --vfat is the only one anyone should have an issue with
	
	yum install dosfstools	#provides mount.vfat and mkfs.vfat
	parted /dev/vdb
		mklabel msdos
		mkpart 1, -1
		q
	mkfs -t vfat /dev/vdb1
	mkdir /mnt/vdb1
	mount -t vfat /dev/vdb1 /mnt/vdb1


------------------------------------------------------------------------------------------------------------
Configure autofs
  --managing file systems ch 18


yum install autofs

vim /etc/auto.master

comment out /misc and /net

     <mount point> <options>   map file

     Add a direct map

         vim /etc/auto.master

             /-    /etc/auto.direct

        vim /etc/auto.direct

             /mnt/share    -fstype=nfs4                srv1:/share

             /mnt/cdrom    -fstype=iso9660,ro    :/dev/cdrom

     Indirect map

         vim /etc/auto.master

             /mnt/dirs           /etc/auto.dirs

         vim /etc/auto.dirs

             *    -fstype=nfs4    /mnt/dirs/&

     Home dir map

         srv&wk    groupadd --gid 2001 jenna

         srv            useradd --uid 2001 --gid 2001 -d /exports/homes/jenna jenna

         wk             useradd --uid 2001 --gid 2001 -d /mnt/homes/jenna -M jenna

         vim /etc/auto.master

             /mnt/homes/    auto.homes

         vim /etc/auto.homes

             *    -fstype=nfs4    srv1:/exports/homes/&

         setsebool use_nfs_home_dirs on -P


-------------------------------------------------------------------------------------------------

Schedule tasks using at

     yum -y install at

     systemctl enable --now atd

     at 16:00 today

     at 12:05 24-04-15

         at> echo happy b-day>>kyle.file

         at>^d

     atq        #Pending at jobs

     atrm <#>        #remove job number listed by atq

------------------------------------------------------------------------------------------------------------
Schedule tasks using cron

     min    hour    dom     month     dow

     crontab -e

         */5 * * * *    /run/job/evrery/5/minutes

         */15 * * * *    /run/job/evrery/15/minutes

         0 */4 * * *    /run/job/every/4/hours

     crontab -l

     crontab -u username -l

     ls /var/spool/cron/

------------------------------------------------------------------------------------------------------------ 
Configure systems to boot into a specific target automatically

	systemctl set-default multi-user.tartget


	rescue.target		#is equivalent to runlevel 1

	multi-user.target	#is equivalent to runlevel 3

	graphical.target	#is equivalent to runlevel 5

	# for a one time change at boot
	At end of grub linux line add 'systemd.unit=<target>'
	or... at the end of grub linux line add 1, 3, or 5

	# change the current session without rebooting
	systemctl isolate multi-user.target  
	or... init 3
	
	systemctl list-units -t target   #shows active targets

------------------------------------------------------------------------------------------------------
Modify system bootloader


     grubby --info

     grubby --set-default-index=<entryFrom--info>

     grubby --update-kernel=ALL --args="<parameter>"

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/system_administrators_guide ch-working_with_the_grub_2_boot_loader sec-Making_Persistent_Changes_to_a_GRUB_2_Menu_Using_the_grubby_Tool.


------------------------------------------------------------------------------------------------------------
Manage SELinux port labels

	semanage port -l |grep http

	semanage port -a -t http_port_t -p tcp 9090





------------------------------------------------------------------------------------------------------------





sudo yum install podman skopeo buildah "@container Management"

     sudo vim /etc/containers/registries.conf  -- add registry.redhat.io

     loginctl show-user djt

     sudo loginctl enable-linger 2000

     sudo semanage fcontext -a -e /var/lib/containers /homes/djt/.login/share/containers

     sudo restorecon -R -v /homes/djt/.login/share/containers

Find and retrieve container images from a remote registry

     podman login registry.redhat.io

     podman search ubi9

     podman pull registry.access.redhat.com/ubi9/ubi-minimal

     podman pull registry.redhat.io/ubi9/ubi-minimal

     podman images

     podman rmi registry.redhat.io/ubi9/ubi-minimal

Inspect container images

     skopeo login registry.redhat.io

     skopeo inspect docker://registry.redhat.io/ubi9/httpd

         search for expose to see ports and services

         The Cmd block shows what command is running

         Environment variables will be at the botton of the output

     podman inspect registry.redhat.io.ubi9/httpd    -    for downloaded images

Perform basic container management such as running, starting, stopping, and listing running containers

     podman run registry.redhat.io/ubi9/ubi-minimal

     podman ps

     podman ps -a

     podman rm <container_name> #Remove exited container by name

     podman rm -a   #remove all exited containers

     podman run -it --name myubi-1 <image-ID>  /bin/bash #this will drop you into a shell in the container

         #look around, do what you want, try microdnf

         #exit

     podman ps -a #will show your container exited

     podman start myubi-1  #Not sure why it stays running, maybe because of the -it in the initial invocation

     podman exec -it myubi-1 /bin/bash  #Re-connect to the containers terminal

     podman run -itd -name myubi-2 <image-id>  #start a container int he background

     podman stop OR podman kill <name>

     podman kill -a

     podman rm -a

Run a service inside a container && Attach persistent storage to a container

     podman run --name httpd-1 -d -p 80:8080 <image>   #to map local port 80 to image port 8080

     podman run --name httpd-2 -d -p 80:8080 -v /home/djt/webdocs:/var/www/html:z <image>


Build a container from a Containerfile

     vim ~.containerFiles/index.html

         <b>nginx test</b>

     vim ~/containerFiles/startup.sh

         #!/bin/bash

         exec /usr/sbin/nginx -g "daemon off;"


     vim ~/containerFiles/nginx.cfile

         FROM registry.redhat.io/ubi9/ubi-minimal:9.1.0

         RUN microdnf -y install nginx

         RUN rm -rf /usr/share/nginx/html/*

         COPY index.html /usr/shae/nginx/html/

         COPY startup.sh /

         EXPOSE 80

         CMD /startup.sh


     podman build . -t nginximg:kjl -f ./nginx.cfile

     podman images

     podman run -d --name kjlnginx -p 4080:80 localhost/nginximg:kjl



Configure a container to start automatically as a systemd service

     mkdir -p ~/.config/systemd/user

     cd          ~/.config/systemd/user

     podman generate systemd --name kjlnginx --files --new

     systemctl --user daemon-reload

     systemctl --user start container-kjlnginx.service

     systemctl --user status container-kjlnginx.service

     podman ps

     systemctl --user enable container-kjlnginx.service
