
# gogs install guide

First cd to /opt then we make dirs to run the application
```
mkdir gogs
```
cd etherpad and either git clone or download the zip file
```
wget https://dl.gogs.io/0.11.91/gogs_0.11.91_linux_amd64.tar.gz

tar -zxf gogs_0.11.91_linux_amd64.tar.gz

mv gogs gogs_yourversion

ln -s  current 
````
create a ordinary user to run gogs
```
useradd -m /opt/etherpad/current/ --shell /bin/sh youruser
```

To give the etherpad user access to dir
```
chown -R youruser:youruser /opt/etherpad/current
```

