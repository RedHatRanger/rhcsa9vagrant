### Setting the default kernel using grubby

* Check the current kernel that's loaded by default, then change it to the older kernel:
```
[root@node2 ~]# grubby --default-kernel
/boot/vmlinuz-5.14.0-284.11.1.el9_2.x86_64
[root@node2 ~]#
[root@node2 ~]# grubby --set-default=/boot/vmlinuz-5.14.0-162.6.1.el9_1.x86_64 
The default is /boot/loader/entries/0ba1be1199e74165a458a3bb0f65fb8f-5.14.0-162.6.1.el9_1.x86_64.conf with index 2 and kernel /boot/vmlinuz-5.14.0-162.6.1.el9_1.x86_64
```

* Now check that the default kernel is selected:
```
[root@node2 ~]# grubby --default-kernel
/boot/vmlinuz-5.14.0-162.6.1.el9_1.x86_64
``
