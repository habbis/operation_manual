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

# To set etherpad i production mode
```
export NODE_ENV=production
```

To start etherpad
```
/opt/etherpad-lite/bin/run.sh
```





