# NFS

Network Files System (NFS).

A bit of history:

Network File System (NFS) is a distributed file system protocol originally developed by Sun Microsystems in 1984, 
allowing a user on a client computer to access files over a network in a manner similar to how local storage is accessed.



Configuration
-------------

Server


Global configuration options are set in /etc/nfs.conf. Users of simple configurations should not need to edit this file.

The NFS server needs a list of exports (see exports(5) for details) which are defined in /etc/exports or /etc/exports.d/*.exports. 
These shares are relative to the so-called NFS root. 
A good security practice is to define a NFS root in a discrete directory tree which will keep users limited to that mount point.
Bind mounts are used to link the share mount point to the actual directory elsewhere on the filesystem.

To let the remote user get access to the share the right permissions.

```
$ mkdir /path/to/share
$ chown youruser:yourgroup /path/to/share
$ chmod 755 /path/to/share
```
If you create an common group or user then that user or group can access the share or set nfsnobody:nfsnobody but 
its not recommend or use the command $ chmod 777 

The way to setup nfs shears should be the same on all Linux Performance Tools systems but on zfs have 
inn build command to share linkt to note: ZFS dataset NFS shearing

```
$ vi /etc/exports 
```
Add this 

```
/yourshare          192.168.1.101(rw,sync,no_root_squash,no_subtree_check)
/path/to/share        192.168.1.101(rw,sync,no_subtree_check)
```
(The no_root_squash option makes that /yourshare will be accessed as root.)

Whenever we modify /etc/exports, we must run:

```
$ exportfs -a
```

Centos7: 

If you have firewalld on the system  you must open for nfs shearing.

```
$ firewall-cmd --permanent --zone=public --add-service=ssh
$ firewall-cmd --permanent --zone=public --add-service=nfs
$ firewall-cmd --reload
```
Next install the nfs server.

```
$ yum install -y nfs-utils
```
Then enable and start the nfs-server service.

```
$ systemctl enable nfs-server.service
$ systemctl start nfs-server.service

```
debian/Ubuntu 18.04: 

```
$ apt install nfs-kernel-server
```

If exportfs wont on ubuntu restart service.

```
$ systemctl restart nfs-kernel-server
```

If you have ufw on the system you most open for nfs shearing.

```
$ ufw allow nfs 
```


Other nfs server stuff 

Consider this following example wherein:

    The NFS root is /srv/nfs.
    The export is /srv/nfs/music via a bind mount to the actual target /mnt/music.
    
```
$ mkdir -p /srv/nfs/music /mnt/music
$ mount --bind /mnt/music /srv/nfs/music
```
Note: ZFS filesystems require special handling of bindmounts, see ZFS#Bind mount.

To make the bind mount persistent across reboots, add it to fstab:

/etc/fstab

```
/mnt/music /srv/nfs/music  none   bind   0   0
```

Client:

To mount manually first create an folder the place you want the 
share to be mounted usual you give it the name of the nfs share.

```
$ mkdir -p /path/to/share
```
Then mount with either dns name or ip address like this.

```
$ mount 192.168.1.100:/home /path/to/share
$ mount 192.168.1.100:/var/nfs /path/to/share
```
Mounting NFS Shares at Boot Time:

```
$ vi /etc/fstab
```

```
192.168.1.100:/home  /path/to/share  nfs      rw,sync,hard,intr  0     0
192.168.1.100:/var/nfs  /path/to/share   nfs      rw,sync,hard,intr  0     0
```
Here, we’re using the same configuration options for both directories with the exception of no_root_squash. Let’s take a look at what each of these options mean:

rw: 

This option gives the client computer 
both read and write access to the volume.
    
sync: 

This option forces NFS to write changes to disk before replying. This results in a more 
stable and consistent environment since the reply reflects the actual state of the remote volume. 
However, it also reduces the speed of file operations.

no_subtree_check: 

This option prevents subtree checking, which is a process where the host must check whether 
the file is actually still available in the exported tree for every request. This can cause many problems when a 
file is renamed while the client has it opened. In almost all cases, it is better to disable subtree checking.
    

no_root_squash: 

By default, NFS translates requests from a root user remotely into a non-privileged user on the server. 
This was intended as security feature to prevent a root account on the client from using the file system of the host as root. 
no_root_squash disables this behavior for certain shares.

```
$ man nfs

```
Centos7: 

Install the nfs client.

```
$ yum install -y nfs-utils 
```
debian/Ubuntu 18.04: 

```
$ apt install nfs-common
```



Sources
-------

Link centos6: https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-18-04

Link centos7: https://www.howtoforge.com/tutorial/setting-up-an-nfs-server-and-client-on-centos-7/

Link ubuntu 18.04:https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-ubuntu-18-04

Link for many Linux Performance Tools distos: https://www.tecmint.com/how-to-setup-nfs-server-in-linux/


