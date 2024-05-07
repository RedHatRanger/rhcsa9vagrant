root@rocky:~# grubby --default-kernel
/boot/vmlinuz-5.14.0-284.11.1.el9_2.x86_64
root@rocky:~# grubby --set-default=/boot/vmlinuz-5.14.0-
vmlinuz-5.14.0-162.6.1.el9_1.x86_64   vmlinuz-5.14.0-284.11.1.el9_2.x86_64  vmlinuz-5.14.0-362.8.1.el9_3.x86_64   
root@rocky:~# grubby --set-default=/boot/vmlinuz-5.14.0-162.6.1.el9_1.x86_64 
The default is /boot/loader/entries/0ba1be1199e74165a458a3bb0f65fb8f-5.14.0-162.6.1.el9_1.x86_64.conf with index 2 and kernel /boot/vmlinuz-5.14.0-162.6.1.el9_1.x86_64
root@rocky:~# grubby --default-kernel
/boot/vmlinuz-5.14.0-162.6.1.el9_1.x86_64
