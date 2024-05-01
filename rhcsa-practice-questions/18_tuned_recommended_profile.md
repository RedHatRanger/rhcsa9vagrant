***On Node2***

# Configure Tuned

### QUESTION #18:
Configure the recommended tuned profile. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #18:

* Install the "tuned" package:
```
[root@node2 ~]# yum install tuned
Installed:
  hdparm-9.62-2.el9.x86_64             python3-linux-procfs-0.7.0-1.el9.noarch
  python3-perf-5.14.0-72.20.1.el9_0.x86_64  python3-pyudev-0.22.0-6.el9.noarch
  tuned-2.18.0-1.el9.noarch
Complete!
```

* Start and enable the ```tuned``` service:
```
[root@node2 ~]# systemctl enable --now tuned
```

* Run ```tuned-adm list```:
```
[root@node2 ~]# tuned-adm list
Available profiles:
- accelerator-performance         - Throughput performance based tuning with disabled high latency operations
- balanced                        - General non-specialized tuned profile
- desktop                         - Optimize for the desktop use-case
- hpc-compute                     - Optimize for HPC compute workloads
- intel-sst                       - Optimize for Intel Speed Select Base Frequency
- latency-performance             - Optimize for deterministic performance at the cost of increased power consumption
- network-latency                 - Optimize for deterministic performance at the cost of increased power consumption, focused on low latency network performance
- network-throughput              - Optimize for streaming network throughput, generally decreases power consumption
- optimize-serial-console         - Optimize for serial console use.
- powersave                       - Optimize for low power consumption
- throughput-performance          - Broadly applicable tuning that provides excellent performance across a variety of common server workloads
- virtual-guest                   - Optimize for running inside a virtual guest
- virtual-host                    - Optimize for running KVM guests
Current active profile: virtual-guest
```

* Run ```tuned-adm recommend```:
```
[root@node2 ~]# tuned-adm recommend
virtual-guest
```

* Set the profile to the one recommended:
```
[root@node2 ~]# tuned-adm profile virtual-host
```
* To check which profile is active:
```
[root@node2 ~]# tuned-adm active
Current active profile: virtual-host
```

* SUCCESS!!

### Additional Comments: 
This lab is setup in such a way that the tuned-adm profile is already on the recommended, which is fine.  You can alter it and then change it back for the sake of this lab.
