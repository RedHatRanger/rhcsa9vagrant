## Study points for the exam (Updated on 2024-05-02):
RHCSA exam candidates should be able to accomplish the tasks below without assistance. These have been grouped into several categories.

### Understand and use essential tools
- Access a shell prompt and issue commands with correct syntax
- Use input-output redirection (>, >>, |, 2>, etc.)
- Use grep and regular expressions to analyze text <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/12_grep_search_string_and_redirect.md">LAB #12</a>
- Access remote systems using SSH (SEE LAB #21)
- Log in and switch users in multiuser targets
- Archive, compress, unpack, and uncompress files using tar, gzip, and bzip2 <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/08_tar_bz2_tar_tgz_archive.md">LAB #8</a>
- Create and edit text files (ALL THE LABS PRETTY MUCH)
- Create, delete, copy, and move files and directories (ALL THE LABS PRETTY MUCH)
- Create hard and soft links [Lab #22](https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/22_soft_links.md)
- List, set, and change standard ugo/rwx permissions
- Locate, read, and use system documentation including man, info, and files in /usr/share/doc <a href="https://youtu.be/QOYgWxTcmjA?si=5FbtuyTdooElUfuk">(See this video)</a>
### Create simple shell scripts
- Conditionally execute code (use of: if, test, [ ], etc.)
- Use Looping constructs (for, etc.) to process file, command line input
- Process script inputs ($1, $2, etc.)
- Processing output of shell commands within a script

### Operate running systems
- Boot, reboot, and shut down a system normally
- Boot systems into different targets manually
- Interrupt the boot process in order to gain access to a system [Lab #13](https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/23_grub_bootloader_password.md)
- Identify CPU/memory intensive processes and kill processes
- Adjust process scheduling
- Manage tuning profiles (SEE LAB #18)
- Locate and interpret system log files and journals
- Preserve system journals
- Start, stop, and check the status of network services
- Securely transfer files between systems

### Configure local storage
- List, create, delete partitions on MBR and GPT disks
- Create and remove physical volumes
- Assign physical volumes to volume groups
- Create and delete logical volumes
- Configure systems to mount file systems at boot by universally unique ID (UUID) or label
- Add new partitions and logical volumes, and swap to a system non-destructively

### Create and configure file systems
- Create, mount, unmount, and use vfat, ext4, and xfs file systems
- Mount and unmount network file systems using NFS <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/00_ansible-nfs_server_configuring.md">Lab #00</a>
- Configure autofs <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/07_autofs_configuring.md">LAB #7</a>
- Extend existing logical volumes <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/17_reduce_or_extend_a_logical_volume%20_size.md">LAB #17</a>
- Create and configure set-GID directories for collaboration <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/05_shared_directory_setup.md">LAB #5</a>
- Diagnose and correct file permission problems

### Deploy, configure, and maintain systems
- Schedule tasks using at and cron (SEE LAB #6)
- Start and stop services and configure services to start automatically at boot
- Configure systems to boot into a specific target automatically
- Configure time service clients
- Install and update software packages from Red Hat Network, a remote repository, or from the local file system
- Modify the system bootloader

### Manage basic networking
- Configure IPv4 and IPv6 addresses <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/01_setup_network_parameters_and_set_hostname.md">Lab #1</a>
- Configure hostname resolution <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/01_setup_network_parameters_and_set_hostname.md">Lab #1</a>
- Configure network services to start automatically at boot <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/01_setup_network_parameters_and_set_hostname.md">Lab #1</a>
- Restrict network access using firewall-cmd/firewall

### Manage users and groups
- Create, delete, and modify local user accounts <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/04_create_users_with_secondary_groups.md">Lab #4</a>
- Change passwords and adjust password aging for local user accounts <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/04_create_users_with_secondary_groups.md">Lab #4</a>
- Create, delete, and modify local groups and group memberships
- Configure superuser access <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/04_create_users_with_secondary_groups.md">Lab #4</a>

### Manage security
- Configure firewall settings using firewall-cmd/firewalld
- Manage default file permissions
- Configure key-based authentication for SSH
- Set enforcing and permissive modes for SELinux
- List and identify SELinux file and process context
- Restore default file contexts
- Manage SELinux port labels <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/03_selinux_fix_add_port_82.md">Lab #3</a>
- Use boolean settings to modify system SELinux settings
- Diagnose and address routine SELinux policy violations

### Manage containers
- Find and retrieve container images from a remote registry <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/20_container_building.md">LAB #20</a>
- Inspect container images <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/20_container_building.md">LAB #20</a>
- Perform container management using commands such as podman and skopeo <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/20_container_building.md">LAB #20</a>
- Build a container from a Containerfile <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/20_container_building.md">LAB #20</a>
- Perform basic container management such as running, starting, stopping, and listing running containers <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/21_container_service.md">LAB #21</a>
- Run a service inside a container <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/21_container_service.md">LAB #21</a>
- Configure a container to start automatically as a systemd service <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/21_container_service.md">LAB #21</a>
- Attach persistent storage to a container <a href="https://github.com/RedHatRanger/rhcsa9vagrant/blob/main/rhcsa-practice-questions/21_container_service.md">LAB #21</a>
- As with all Red Hat performance-based exams, configurations must persist after reboot without intervention. 

## ***Red Hat reserves the right to add, modify, and remove objectives. Such changes will be made public in advance through revisions to this document:***
## https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam?section=objectives
<br/><br/><br/>
Purchase the EX200 Exam here:
https://www.redhat.com/en/services/training/ex200-red-hat-certified-system-administrator-rhcsa-exam \
AND to schedule click here:
https://rhtapps.redhat.com/individualexamscheduler/#/

* What the exam scheduler looks like:
![image](https://github.com/RedHatRanger/rhcsa9vagrant/assets/90477448/b2f582ab-12dc-42da-900b-b0d96373bb08)
