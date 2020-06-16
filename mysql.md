
# Installing mysql

## centos/RHEL

centos8/RHEL8 repo
wget https://dev.mysql.com/get/mysql80-community-release-el8-1.noarch.rpm


centos7/RHEL7 repo
wget https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm





## ubuntu/debian
```
sudo apt update
sudo apt install mysql-server

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
[mysql community downloads](https://dev.mysql.com/downloads/repo/yum/)
[A Quick Guide to Using the MySQL Yum Repository](https://dev.mysql.com/doc/mysql-yum-repo-quick-guide/en/)
