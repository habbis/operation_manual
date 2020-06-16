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
## mysql java database connector 
confluence does not shipt with mysql support you must add the [mysql java connetor](https://dev.mysql.com/downloads/connector/j/) 

Dowload tar file and get it to the server
```
wget https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-8.0.20.tar.gz
```

Then extract the tar file and copy it the right confluence directory
```
tar -zxf mysql-connector-java-8.0.20.tar.gz 

mysql-connector-java-8.0.20 /opt/confluence/current/confluence/WEB-INF/lib
```


## Configuring MySQL/Mariadb for confluence

Here is all the recommended changes in [doc](https://confluence.atlassian.com/doc/database-setup-for-mysql-128747.html)

```
[mysqld]

character-set-server=utf8mb4

collation-server=utf8mb4_bin

default-storage-engine=INNODB

max_allowed_packet=256M

innodb_log_file_size=2GB

```

Ensure that the global transaction isolation level of your Database had been set to READ-COMMITTED.
```
...
transaction-isolation=READ-COMMITTED
```

Check that the binary logging format is configured to use 'row-based' binary logging.

```
binlog_format=row
```

remove this if it exists
```
sql_mode = NO_AUTO_VALUE_ON_ZERO

```


## External links:

[confluence install guide](https://confluence.atlassian.com/doc/installing-confluence-on-linux-from-archive-file-255362363.html)
[confluence download page](https://www.atlassian.com/software/confluence/download)
