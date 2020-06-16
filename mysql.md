
# Installing mysql

## centos/RHEL

centos8/RHEL8 repo
```
wget https://dev.mysql.com/get/mysql80-community-release-el8-1.noarch.rpm
```

repo managment on centos8 here you can use both yum and dnf
```
sudo dnf config-manager --disable mysql80-community
sudo dnf config-manager --enable mysql57-community
```

centos7/RHEL7 repo
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm


repo managment centos7 
```
sudo yum-config-manager --disable mysql80-community
sudo yum-config-manager --enable mysql57-community
```



## ubuntu/debian

ubuntu/debian repo
```
wget https://dev.mysql.com/get/mysql-apt-config_0.8.15-1_all.deb
```
To install deb package
```
sudo dpkg -i /mysql-apt-config_0.8.15-1_all.deb
```
Then to install 
```
sudo apt-get update
sudo apt-get install mysql-server
```


This will take you through a series of prompts where you can make some changes to 
your MySQL installation’s security options.
Run the security script:
```
sudo mysql_secure_installation
```


Configuration


External links:
[Database Setup For MySQL](https://confluence.atlassian.com/doc/database-setup-for-mysql-128747.html)
[mysql-on-ubuntu-18-04)](https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-18-04)
[mysql community downloads RHEL/centos](https://dev.mysql.com/downloads/repo/yum/)
[A Quick Guide to Using the MySQL Yum Repository](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/)
[mysql community downloads debian/ubuntu](https://dev.mysql.com/downloads/repo/apt/)
[MySQL Community Downloads](https://dev.mysql.com/downloads/)
