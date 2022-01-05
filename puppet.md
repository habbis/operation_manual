# puppet notes 


## setup puppet server.

First find the right repo for your distro.


#### puppet 7

ubuntu 20.04
```
wget https://apt.puppet.com/puppet7-release-focal.deb
```

debian 11
```
wget https://apt.puppet.com/puppet7-release-bullseye.deb
```

rhel 8
```
https://yum.puppet.com/puppet7-release-el-8.noarch.rpm
```

#### puppet 6

ubuntu 20.04
```
wget https://apt.puppet.com/puppet6-release-focal.deb
```

debian 11
```
wget https://apt.puppet.com/puppet6-release-bullseye.deb
```

rhel 8
```
https://yum.puppet.com/puppet6-release-el-8.noarch.rpm
```

Then to install puppet server.

Ubuntu and debian.
```
apt install -y puppetserver puppet-agent
```

rhel 8
```
yum install -y puppetserver puppet-agent
```

Add  fqdn to hosts file.
```
vim /etc/hosts
```

Then add it like this.
```
27.0.0.1       localhost.localdomain   localhost
yourip   puppet.local.net     puppet
```

Then edit puppet.conf .
```
vim /etc/puppetlabs/puppet/puppet.conf
```

Add the fqdn of your puppet server.
```
[main]
server = puppet.local.net
```

Then on ubuntu and debian restart puppet server you should see no errors.

```
systemctl restart puppetserver.service
```

On rhel you must enable and start the puppet server service here 
is the one liner.

```
systemctl enable --now puppetserver.service
```

Then you add puppet classes. 




