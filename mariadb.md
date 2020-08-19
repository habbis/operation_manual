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
sudo yum install -y  MariaDB-server
sudo systemctl enable --now mariadb
```


centos8
```
sudo dnf install -y MariaDB-server
sudo systemctl enable --now mariadb
```

## Installing mariadb backup

centos7
```
sudo yum install -y  MariaDB-backup
```

centos8
```
sudo dnf install -y MariaDB-backup
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
links:

https://computingforgeeks.com/how-to-setup-mariadb-galera-cluster-on-debian/

https://computingforgeeks.com/galera-cluster-high-availability-with-haproxy-on-ubuntu-18-04-centos-7/

https://mariadb.org/download/