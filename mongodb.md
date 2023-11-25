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
