# mongodb

## setup mongodb 7 community edition on debian 11

```
sudo apt-get install gnupg curl
```

To import the MongoDB public GPG key from https://pgp.mongodb.com/server-7.0.asc, run the following command.

Add gpg key

```
curl -fsSL https://pgp.mongodb.com/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
```

Add repo

```
echo "deb [ signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] http://repo.mongodb.org/apt/debian bullseye/mongodb-org/7.0 main" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
```

Install
```
sudo apt-get update ; sudo apt-get install -y mongodb-org
```

Enable and start mongodb service
```
sudo systemctl enable --now  mongod
```

To use the mongodb shell
```
mongosh
```

Creating Administrative MongoDB User

Login into mongodb shell 

```
mongo
```

## securing mongodb

From inside the MongoDB shell, type the following command to connect to the admin database
```
use admin
```

Issue the following command to create a new user named mongoAdmin with the userAdminAnyDatabase role
```
db.createUser(
  {
    user: "mongoAdmin", 
    pwd: "changeMe", 
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)
```
To list users
```
show users
```

To test the changes, access the mongo shell using the administrative user you have previously created:
```
mongosh -u mongoAdmin -p --authenticationDatabase admin
```

To enable authorization edit /etc/mongod.conf
```
vim /etc/mongod.conf
```
Add this under security and uncomment
```
security:
  authorization: enabled
```

## remote connection over the network

If you want to enable connection to mongodb over the network change
```
net:
  port: 27017
  bindIp: 127.0.0.1
```
To either listen on spesific ip address or all interfaces

Spesific ip
```
net:
  port: 27017
  bindIp: 192.168.1.2
```

All interfaces 
```
net:
  port: 27017
  bindIp: 0.0.0.0
```

Example mongodb.conf
```
# mongod.conf

# for documentation of all options, see:
#   http://docs.mongodb.org/manual/reference/configuration-options/

# Where and how to store data.
storage:
  dbPath: /var/lib/mongodb
#  engine:
#  wiredTiger:

# where to write logging data.
systemLog:
  destination: file
  logAppend: true
  path: /var/log/mongodb/mongod.log

# network interfaces
net:
  port: 27017
  bindIp: 0.0.0.0


# how the process runs
processManagement:
  timeZoneInfo: /usr/share/zoneinfo

security:
  authorization: enabled

#operationProfiling:

#replication:

#sharding:

## Enterprise-Only Options:

#auditLog:

```





Links:
https://www.mongodb.com/docs/v7.0/tutorial/install-mongodb-on-debian/
https://linuxize.com/post/how-to-install-mongodb-on-debian-10/



