# etherpad install guide

First cd to /opt then we make dirs to run the application
```
mkdir etherpad
mkdir node
```
cd etherpad and either git clone or download the zip file
```
git clone --branch master git://github.com/ether/etherpad-lite.git

ln -s etherpad-lite current 
````
Or we use the zip file
```
https://github.com/ether/etherpad-lite/archive/1.8.4.zip

unzip 1.8.4.zip

ln - etherpad-lite-1.8.4 current
```

create a ordinary user to run etherpad
```
useradd --create-home --shell /bin/bash youruser
```

creat a user with not shell
```
sudo useradd -m --shell /bin/nologin youruser
```
To give the etherpad user access to dir
```
chown -R youruser:youruser /opt/etherpad/current
```
To test run etherpad 
```
sudo su - youruser -s /bin/sh -c /opt/etherpad-lite/bin/run.sh
```
cd node then download nodejs lts
```
wget https://nodejs.org/dist/v12.16.3/node-v12.16.3-linux-x64.tar.xz

# if missing xf-utils 
 apt install -y  xz-utils
 
tar xJvf node-v12.16.3.tar.xz

ln -s node-v12.16.3 current  
```
Then we symlink npm nad node in working dir
```
ln -s /opt/node-v12.16.3-linux-x64/bin/npm /usr/bin/npm 
ln -s /opt/node-v12.16.3-linux-x64/bin/node /usr/bin/node
```

 To set etherpad i production mode
```
export NODE_ENV=production
```

To start etherpad
```
/opt/etherpad-lite/bin/run.sh
```

It will create a settings.json at 

```
ls -al /opt/etherpad-lite/
total 205
drwxr-xr-x 10 user user    25 May 21 20:57 .
drwxr-xr-x  4 root root     5 May 21 20:49 ..
-rw-r--r--  1 user user   424 May 21 20:49 .dockerignore
drwxr-xr-x  8 user user    13 May 21 20:49 .git
drwxr-xr-x  3 user user     4 May 21 20:49 .github
-rw-r--r--  1 user user   334 May 21 20:49 .gitignore
-rw-r--r--  1 user user  1468 May 21 20:49 .travis.yml
-rw-r--r--  1 user user    64 May 21 20:57 APIKEY.txt
-rw-r--r--  1 user user 29049 May 21 20:49 CHANGELOG.md
-rw-r--r--  1 user user  8217 May 21 20:49 CONTRIBUTING.md
-rw-r--r--  1 user user  1642 May 21 20:49 Dockerfile
-rw-r--r--  1 user user 11353 May 21 20:49 LICENSE
-rw-r--r--  1 user user   837 May 21 20:49 Makefile
-rw-r--r--  1 user user  7493 May 21 20:49 README.md
-rw-r--r--  1 user user    64 May 21 20:57 SESSIONKEY.txt
drwxr-xr-x  4 user user    27 May 21 20:49 bin
drwxr-xr-x  6 user user    16 May 21 20:49 doc
drwxr-xr-x  2 user user     3 May 21 20:56 node_modules
-rw-r--r--  1 user user 16790 May 21 20:56 settings.json
-rw-r--r--  1 user user 18740 May 21 20:49 settings.json.docker
-rw-r--r--  1 user user 16790 May 21 20:49 settings.json.template
drwxr-xr-x  7 user user    14 May 21 20:57 src
-rw-r--r--  1 user user    51 May 21 20:49 start.bat
drwxr-xr-x  5 user user     6 May 21 20:49 tests
drwxr-xr-x  2 user user    10 May 21 21:27 var
```



