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

Centos/RHEL
```
yum install java-11-openjdk-headless
yum install fontconfig
```
ubuntu/debian
```
apt install -y openjdk-11-jdk-headless
apt install -y fontconfig
```

## Installation

Download
Download the release " TAR.GZ " package from Atlassian for the version you intend to run. Do not use the " Linux 64 Bit " installer.

Application
Unpack release tarball to /opt/confluence
```
mkdir /opt/confluence
cd /opt/confluence
tar -zxf atlassian-confluence-7.2.0.tar.gz
```
Symlink the " current " directory to the version you unpacked (this is to avoid making changes to init script etc. during upgrades, and for easier
rollback
```
ln -s atlassian-confluence-7.2.0 current
```
Correct permissions for the installation. It is recommended to have everything owned by root, except three directories which needs write
permission.

```
chown -R root.root /opt/confluence/current/
chown $CONFUSER /opt/confluence/current/conf/Standalone/localhost
chown $CONFUSER /opt/confluence/current/temp
chown $CONFUSER /opt/confluence/current/work
```
Create log directory under /var/log and replace the logs directory under /opt with a symlink to it.
```
mkdir /var/log/confluence
chown $CONFUSER /var/log/confluence
chmod 700 /var/log/confluence
rm -rf /opt/confluence/current/logs
ln -s /var/log/confluence /opt/confluence/current/logs
```
Create data directory on /var/confluence partition.

```
mkdir /var/confluence/data
chown $CONFUSER /var/confluence/data
chmod 700 /var/confluence/data
```
Create init script as /etc/init.d/confluence . Remember to update the USER variable.
```
#!/bin/sh -e
# Confluence startup script
#chkconfig: 2345 80 05
#description: Confluence
# Define some variables
# Name of app ( JIRA, Confluence, etc )
APP=confluence
# Name of the user to run as
USER=<customerprefix>-wiki
# Location of Confluence install directory
CATALINA_HOME=/opt/confluence/current
# Location of Java JDK
export JAVA_HOME=/etc/alternatives/jre_11_openjdk
case "$1" in
# Start command
start)
echo "Starting $APP"
/bin/su -m $USER -c "$CATALINA_HOME/bin/start-confluence.sh &>
/dev/null"
;;
# Stop command
stop)
echo "Stopping $APP"
/bin/su -m $USER -c "$CATALINA_HOME/bin/stop-confluence.sh &>
/dev/null"
echo "$APP stopped successfully"
;;
# Restart command
restart)
$0 stop
sleep 5
$0 start
;;
*)
echo "Usage: /etc/init.d/$APP {start|restart|stop}"
exit 1
;;
esac
exit 0
```

Make init script executable and add it to start at boot.

```
chmod +x /etc/init.d/confluence
chkconfig --add confluence
```
Mysql database setup

ubuntu 
```
sudo apt update
sudo apt install mysql-server

```

Configuring MySQL

This will take you through a series of prompts where you can make some changes to 
your MySQL installation’s security options.
Run the security script:
```
sudo mysql_secure_installation
```





Configuration


Links:
[Database Setup For MySQL](https://confluence.atlassian.com/doc/database-setup-for-mysql-128747.html)
[mysql-on-ubuntu-18-04)](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04)