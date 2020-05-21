# etherpad install guide

first cd to /opt 
```
mkdir etherpad
mkdir node
```
cd etherpad
```
git clone --branch master git://github.com/ether/etherpad-lite.git

ln -s etherpad-lite current 
```
cd node then download nodejs lts
```
wget https://nodejs.org/dist/v12.16.3/node-v12.16.3.tar.gz

tar -zxf node-v12.16.3.tar.gz 

ln -s node-v12.16.3 current  
```




