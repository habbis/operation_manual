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

Successfully added user: {
	"user" : "mongoAdmin",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	]
}

```

To test the changes, access the mongo shell using the administrative user you have previously created:
```
mongo -u mongoAdmin -p --authenticationDatabase admin
```


Links:
https://www.mongodb.com/docs/v7.0/tutorial/install-mongodb-on-debian/
https://linuxize.com/post/how-to-install-mongodb-on-debian-10/



