# system restore login rhel

When at the Grub menu and selecting the boot kernel press `e` .
then at the end on the line starting with linux then press `End` if it dont work just use the arror keys to get to the end . 
Then append `rd.break` at the end of the line and if you also 
add `enforcing 0` it enables omitting the time consuming SElinux relabeling process.
Then press `Ctrl+x` to boot using the modified config.

Then if you have and encrypted hdd it will prompt you for the password
and when at the command prompt use these commands to login to the system.

If running SEllinux on the system remmember to create .autorelabel if you making anny 
changes. 

```
mount -o remount,rw /sysroot
chroot /sysroot

do your changes 

```
Then after anny changes 
```
cd 
touch .autorelabel
```
