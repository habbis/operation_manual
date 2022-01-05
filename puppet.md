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

Add puppet bin to path.

```
echo 'export PATH=$PATH:/opt/puppetlabs/bin/' >> /etc/profile
```

Add  fqdn to hosts file.
```
vim /etc/hosts
```

Then add it like this.
```
127.0.0.1       localhost.localdomain   localhost
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

Adding node to puppet server.

Use the same repos as when installing puppet server but only install 
puppet agent .

Add puppet bin to path.

```
echo 'export PATH=$PATH:/opt/puppetlabs/bin/' >> /etc/profile
```

Ubuntu and debian.
```
apt install -y puppet-agent
```

rhel 8
```
yum install -y puppet-agent
```

Then on ubuntu and debian restart puppet server you should see no errors.

```
systemctl restart puppet
```

On rhel you must enable and start the puppet server service here 
is the one liner.

```
systemctl enable --now puppet
```

Then edit puppet.conf .
```
vim /etc/puppetlabs/puppet/puppet.conf
```

Add the fqdn of your puppet server .
```
[main]
server = puppet.local.net
```


Then you add puppet classes. 

I like to add my custom classes to 

```
/etc/puppetlabs/code/modules/
```

To install puppet class from puppet [forge](https://forge.puppet.com/).

```
puppet module uninstall puppetlabs-accounts
  
```

Then to remove.

```
puppet module uninstall ploperations-account
```

To list installed modules.

```
puppet module list
```

To create a inventory.

```
vim /etc/puppetlabs/code/environments/production/manifests/site.pp
```

How inventory looks like.

after adding a node add your classes inside {} .

```
node 'puppet.local.net' {

  # Configure puppetdb and its underlying database
  class { 'puppetdb': }

  # Configure the Puppet master to use puppetdb
  class { 'puppetdb::master::config': }

  class { "puppet_homelab::server_lite": }

  class { 'os_patching': }

  class { "puppet_homelab::container_user": }

  class { "puppet_homelab::git_user": }

  class { "habbfarm::remove_user": }

  class { 'docker':
  docker_users => ['cusr'],

  }



}


node 'dev01.local.net' {

  class { "puppet_homelab::server_lite": }

  class { 'os_patching': }



}
```











