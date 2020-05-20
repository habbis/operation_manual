# Confluence install guide 

Create a user to run confluence
```
useradd --home-dir /opt/confluence --shell /bin/nologin youruser
```

## Prepare shell
For convenience
```
CONFUSER=youruser
```


## Partitioning
Confluence will be installed under /opt , the minimum recommended size is 5G to have space for multiple versions during upgrades.
```
lvextend -rL5G /dev/vg0/Opt
```

It is strongly recommended to place the Confluence data directory on its own partition. Ideally also on a different volume group than the OS, this
example is for vg1
```
lvcreate -n VarConfluence -L10G vg1
mkfs.xfs /dev/mapper/vg1-VarConfluence
echo "/dev/mapper/vg1-VarConfluence /var/confluence xfs defaults 0 0" >> /etc/fstab
mkdir /var/confluence
mount -a
```
## Prerequisites

Java and Fonts
Newer versions of Confluence support OpenJDK, and this is the recommended JVM to use.
Confluence requires some fonts installed on the server. The easiest way to get them is just to install fontconfig.
```
yum install java-11-openjdk-headless
yum install fontconfig
```





