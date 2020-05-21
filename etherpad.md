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
wget https://nodejs.org/dist/v12.16.3/node-v12.16.3-linux-x64.tar.xz

# if missing xf-utils 
 apt install -y  xz-utils
 
tar xJvf node-v12.16.3.tar.xz

ln -s node-v12.16.3 current  
```


tar xJvf

