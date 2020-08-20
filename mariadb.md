# Setup mariadb centos 

## Setup repo 

centos7

`vim /etc/yum.repos.d/MariaDB.repo`
```
# MariaDB 10.5 [Stable] CentOS repository list - created 2020-08-18 23:25 UTC
# https://mariadb.org/download-test/
[mariadb]
name = MariaDB
baseurl = http://mirror.terrahost.no/mariadb/yum/10.5/centos7-amd64
gpgkey=http://mirror.terrahost.no/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck=1


```
centos8
```
# MariaDB 10.5 [Stable] CentOS repository list - created 2020-08-19 21:28 UTC
# https://mariadb.org/download-test/
[mariadb]
name = MariaDB
baseurl = http://mirror.terrahost.no/mariadb/yum/10.5/centos8-amd64
module_hotfixes=1
gpgkey=http://mirror.terrahost.no/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck=1
```
Adding gpg key

```
sudo rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```

## If you want to install just mariadb

centos7
```
 yum install -y  MariaDB-server
 systemctl enable --now mariadb
```
Securing database
```
mysql_secure_installation
```

centos8
```
 dnf install -y MariaDB-server
 systemctl enable --now mariadb
```

## Installing mariadb backup

centos7
```
 yum install -y  MariaDB-backup
```

centos8
```
 dnf install -y MariaDB-backup
```

## To setup mariadb Galera Cluster (maste master cluster)


centos7
```
yum install -y  MariaDB-server MariaDB-client galera-4
```
cento8
```
dnf install -y  MariaDB-server MariaDB-client galera-4
```

Then run galera new cluster to generate config that you need to edit 
```
 galera_new_cluster
```

Setup storage on a seperate partition for db storage with lvm and xfs filesytem
```
pvcreate /dev/sdb

vgcreate vgdb /dev/sdb

lvcreate -n lvdb -L +49G vgdb

 mkfs.xfs /dev/vgdb/lvdb
```
Stop database and backup the datadir of mariadb
```
 systemctl stop mariadb

 cp -au  /var/lib/mysql/ /var/lib/bk.mysql
 
 rm -rf  /var/lib/mysql/
```
Then mount the lvvol in fstab and test mount it 

`vim /etc/fstab`

append at the end 

```
 database
/dev/mapper/vgdb-lvdb /var/lib/mysql xfs defaults 0 0
```
Test mount and copy files from backup and give right permissions

```
mount -a 

cp -au /var/lib/bk.mysql/* /var/lib/mysql/

chown -R mysql:mysql /var/lib/mysql

```

Setup cluster confg `vim /etc/my.cnf.d/server.cnf`
```
#
# These groups are read by MariaDB server.
# Use it for options that only the server (but not clients) should see
#
# See the examples of server my.cnf files in /usr/share/mysql/
#

# this is read by the standalone daemon and embedded servers
[server]

# this is only for the mysqld standalone daemon
[mysqld]

#
# * Galera-related settings
#
[galera]
# Mandatory settings
wsrep_on=ON
wsrep_provider=/usr/lib64/galera-4/libgalera_smm.so
wsrep_cluster_name="dbcluster01"
wsrep_cluster_address="gcomm://"
# ip if host
wsrep_node_address="10.2.1.2"
binlog_format=row
default_storage_engine=InnoDB
innodb_autoinc_lock_mode=2
#
# Allow server to accept connections on all interfaces.
#
bind-address=0.0.0.0
#
# Optional setting
#wsrep_slave_threads=1
#innodb_flush_log_at_trx_commit=0

# this is only for embedded server
[embedded]

# This group is only read by MariaDB servers, not by MySQL.
# If you use the same .cnf file for MySQL and MariaDB,
# you can put MariaDB-only options here
[mariadb]

# This group is only read by MariaDB-10.5 servers.
# If you use the same .cnf file for MariaDB of different versions,
# use this group for options that older servers don't understand
[mariadb-10.5]

```
Securing databse can you do in the beggining or after cluster config setup
`mysql_secure_installation`

Just to be safe run glera new cluste scrip
`galera_new_cluster`


Start the database
`systemctl enable --now mariadb.service`
Or restart if you have enabled
`systemctl restart mariadb.service`

second node follow all the above cluster steps but config is a bit diffren.
Under wsrep_cluster_address you add both ip addresses and with a tird node do the 
same but add another ip that is that nodes.
```
#
# These groups are read by MariaDB server.
# Use it for options that only the server (but not clients) should see
#
# See the examples of server my.cnf files in /usr/share/mysql/
#

# this is read by the standalone daemon and embedded servers
[server]

# this is only for the mysqld standalone daemon
[mysqld]

#
# * Galera-related settings
#
[galera]
# Mandatory settings
wsrep_on=ON
wsrep_provider=/usr/lib64/galera-4/libgalera_smm.so
wsrep_cluster_name="dbcluster01"
wsrep_cluster_address="gcomm://10.2.1.2,10.2.1.3"
wsrep_node_address="10.2.1.3"
binlog_format=row
default_storage_engine=InnoDB
innodb_autoinc_lock_mode=2
#
# Allow server to accept connections on all interfaces.
#
bind-address=0.0.0.0
#
# Optional setting
#wsrep_slave_threads=1
#innodb_flush_log_at_trx_commit=0

# this is only for embedded server
[embedded]

# This group is only read by MariaDB servers, not by MySQL.
# If you use the same .cnf file for MySQL and MariaDB,
# you can put MariaDB-only options here
[mariadb]

# This group is only read by MariaDB-10.5 servers.
# If you use the same .cnf file for MariaDB of different versions,
# use this group for options that older servers don't understand
[mariadb-10.5]

```

Login to mariadb and check if cluster is up and running 
`how status like 'wsrep_%';`

Then it will show detailed info but its online if you see this 
`| wsrep_connected               | ON`

## Configuration

Create database 
```
create database `yourdb`;
```

Create user for database 

for local db user localhost
```
create user `gogs`@localhost identified by 'yourpass';
```
for remote login you can set with ip or blank for a cluster 
```
create user `gogs`@ identified by 'yourpass';
```
```
create user `gogs`@10.2.1.2 identified by 'yourpass';
```

Delete user
```
drop user `gogs`@yourip;
```
remove user when ip is blank
```
drop user `gogs`@;
```
delete database
```
drop database;
```
Grant user privileges to database
```
grant all privileges on `gogs` to `gogs`@;
```
Show what your user privileges
```
show grants for 'gogs'@;
```
Show database
```
show databases;
```


links:

https://computingforgeeks.com/how-to-setup-mariadb-galera-cluster-on-debian/

https://computingforgeeks.com/galera-cluster-high-availability-with-haproxy-on-ubuntu-18-04-centos-7/

https://mariadb.org/download/
