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

Make the change active.

```
source /etc/profile
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

### puppet agent

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

To add customer runtime of puppet in puppet.conf and
the default is 30m 
```
[main]
server = puppet.local.net
# runinterval = 30m
runinterval = 1h

```

To generate puppet client signing request so you can 
do changes to the node.

This command is also the same as running puppet manualy.
```
/opt/puppetlabs/bin/puppet agent -tv
```
If you are having problems delete ssl folder and 
crate new sign request.

```
rm -rf /etc/puppetlabs/puppet/ssl
```

Then on the puppet server you can list the request.

```
/opt/puppetlabs/bin/puppetserver ca --list
```

Then sign the request.

```
/opt/puppetlabs/bin/puppetserver ca sign --certname dev01.local.net,you-can-sign-more-at-the-same-time
```

Then on the client run the puppet agent command again
so it will complete the setup between client and server.

```
/opt/puppetlabs/bin/puppet agent -t
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

### puppet environtments

Puppet environtments can be used to isolate host more logicaly like having diffrent customer per environtments. But they hare not hundred percent isolated. If dev and prod should not be mixed have seperate puppet servers instead.


Adding custom environtments.

```
cp /etc/puppetlabs/code/environments/production /etc/puppetlabs/code/environments/dev

```

To create a inventory.

```
vim /etc/puppetlabs/code/environments/dev/manifests/site.pp
```

Then you have new environtments if you have added nothing to production
then there is no config that will make problem for you.


What module paths  dev environment have access by default is these.
````
/etc/puppetlabs/code/environments/dev/modules
/etc/puppetlabs/code/modules
/opt/puppetlabs/puppet/modules
````

You can check it with this command.
```
puppet config print modulepath --section server --environment dev
```

If you want dev access production you can add this to environment.conf .
```
vim environment.conf
```
At the end of the file addd.
```
modulepath =  /etc/puppetlabs/code/environments/production/modules:modules:$basemodulepath
```

### puppet modules 

Tp add puppet classes. 

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

Then to enable client to run on diffrent environments add this to puppet.conf .
```
 cat /etc/puppetlabs/puppet/puppet.conf
```
Then add this environments under main.
```
 [main]
server = puppet.local.net
environment = dev
runinterval = 30m
```


Nice puppet module to have
```
 # manage puppetdb
 puppet module install puppetlabs-puppetdb
 
 # manage docker
 puppet module install puppetlabs-docker
 
 # manage mysql
 puppet module install puppetlabs-mysql
```

Some of my own.


- [puppet_homelab](https://github.com/habbis/puppet_homelab])
- [pve](https://github.com/habbis/pve)






