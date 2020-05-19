# lxd

Here is a guide to setup lxd. warning this guide can be outdated a new guide will maybe come late.
[lxd setup](https://habbis.github.io/post/lxd/)

ways to user cloud-init to setup lxc container 
```

So there are multiple ways you can do so, either directly for a container:

lxc init ubuntu: CONTAINER
lxc config set CONTAINER user.user-data - < cloud-init-config.yml
lxc start CONTAINER

Or even shorter:

lxc launch ubuntu: CONTAINER --config=user.user-data="$(cat cloud-init-config.yml)"

Or through a profile:

lxc profile create dev
lxc profile set dev user.user-data - < cloud-init-config.yml
lxc launch ubuntu: CONTAINER -p default -p dev


```
run docker inside a lxc container managed by lxd use nesting and this is a similar thing like vm nesting but this is not hardware based like with a hardware virtualization.

To launch a container with nesting 

```
lxc launch ubuntu nestc1 -c security.nesting=true 
```
If you want a priviliged container(its not recommended to user privliged)
```
lxc launch ubuntu nestc1 -c security.nesting=true -c security.privileged=true
```
You may need to add more UIDs and GIDs to lxd so see
[Custom user mappings in LXD containers](https://stgraber.org/2017/06/15/custom-user-mappings-in-lxd-containers/)

Here is what is look like.

```
stgraber@castiana:~$ cat /etc/subuid
lxd:100000:65536
root:100000:65536

stgraber@castiana:~$ cat /etc/subgid
lxd:100000:65536
root:100000:65536

The maps for “lxd” and “root” should always be kept in sync. LXD itself is restricted by the “root” allocation. The “lxd” entry is used to track what needs to be removed if LXD is uninstalled.

Now if you want to increase the size of the map available to LXD. Simply edit both of the files and bump the last value from 65536 to whatever size you need. I tend to bump it to a billion just so I don’t ever have to think about it again:

stgraber@castiana:~$ cat /etc/subuid
lxd:100000:1000000000
root:100000:1000000000

stgraber@castiana:~$ cat /etc/subgid
lxd:100000:1000000000
root:100000:100000000
```


links:

[configure lxd containers with cloud config](https://askubuntu.com/questions/617865/is-there-a-way-to-configure-lxd-containers-with-cloud-config-at-provision-time)
[Nested containers in LXD](https://ubuntu.com/blog/nested-containers-in-lxd)
