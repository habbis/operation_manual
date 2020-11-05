# setup postgres 13

## setup repos 


### centos 7 

```
sudo yum install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo yum install -y postgresql13-server
sudo /usr/pgsql-13/bin/postgresql-13-setup initdb
sudo systemctl enable postgresql-13
sudo systemctl start postgresql-13

```

### centos8

```
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo dnf -qy module disable postgresql
sudo dnf install -y postgresql13-server
sudo /usr/pgsql-13/bin/postgresql-13-setup initdb
sudo systemctl enable postgresql-13
sudo systemctl start postgresql-13

```

### ubuntu
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql

```

### debian
```
sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install postgresql

```
### storage 


Setup storage on a seperate partition for db storage with lvm and xfs filesytem
```
pvcreate /dev/sdb

vgcreate vgdb /dev/sdb

lvcreate -n lvdb -L +49G vgdb

 mkfs.xfs /dev/vgdb/lvdb
```
Stop database and backup the datadir of mariadb
```
 systemctl stop postgresql-13

 cp -au  /var/lib/pgsql /var/lib/bk.pgsql 
 
 rm -rf  /var/lib/pgsql
```
Then mount the lvvol in fstab and test mount it 

`vim /etc/fstab`

append at the end 

```
 database
/dev/mapper/vgdb-lvdb /var/lib/pgsql  xfs defaults 0 0
```
Test mount and copy files from backup and give right permissions

```
mount -a 

cp -au /var/lib/bk.pgsql * /var/lib/pgsql

chown -R postgres:postgres /var/lib/pgsql



### allow what address postgres instance should listen to

The config file location 
/var/lib/pgsql/13/data/postgresql.conf

Change this line `listen_addresses` to ip address or localhost or '*' for all .

```
listen_addresses = '*'		# what IP address(es) to listen on;
					# comma-separated list of addresses;
					# defaults to 'localhost'; use '*' for all
```


### allowing remote connection

Edit this file vim  /var/lib/pgsql/13/data/pg_hba.conf and the lines at the end

To allow password auth from spesific ip adress you can do it like 
```
host   all             all             10.21.2.2/32             scram-sha-256

```
 trust option allow connection without password

```
host   all             all             10.21.2.2/32              trust

```

Then restart postgres service
```
systemctl restart postgresql-13
```

### To create database

first login to postgres user
```
su - postgres
```
```
To login to postgres cli 
```
psql
```
Create database with owner
```
CREATE DATABASE etherpad WITH ENCODING='UTF8' OWNER=etherpad;
```
Create user 
```
 CREATE ROLE etherpad WITH LOGIN PASSWORD 'yourpass' VALID UNTIL 'infinity';
```

```
show databases
```
\l+
```

to show storage dir
```
show data_directory;
```






