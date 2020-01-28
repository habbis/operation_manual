
# Networking for servers


## Linux networking 

Manage Linux networking.


- Ubuntu, Debian 

ifupdown(via networking-manager), netplan(ubuntu), network-manager

Setup

ifupdown

sudo apt install -y ifupdown network-manager

vim /etc/NetworkManager/NetworkManager.conf 

It will look like this and remmember to add auto if you dont the inerface will not 
enable networking at boot.

```
[main]
plugins=ifupdown,keyfile

[ifupdown]
managed=false

[device]
wifi.scan-rand-mac-address=no

```
Change the line ifupdown to true

```
[ifupdown]
managed=true

```
Then remove netplan if you want.

`sudo apt remove -y --purge netplan.io`

Then to setup the networking.

`vim /etc/network/interfaces`

This is a example config
```
# ifupdown has been replaced by netplan(5) on this system.  See
# /etc/netplan for current configuration.
# To re-enable ifupdown on this system, you can run:
#    sudo apt install ifupdown
#
# The loopback network interface
auto lo
iface lo inet loopback

#allow-hotplug enp0s3
#auto enp0s3
#iface enp0s3 inet static
  #address 172.16.3.2
  #netmask 255.255.255.0
  #broadcast 172.16.3.255
  #gateway 172.16.3.1
  ## Only relevant if you make use of RESOLVCONF(8)
  ## or similar...
  #dns-nameservers 1.1.1.1 1.0.0.1
  #

#allow-hotplug enp0s3
auto enp129s0f1
iface enp129s0f1 inet static
  address 172.16.3.2
  netmask 255.255.255.0
  broadcast 172.16.3.255
  gateway 172.16.3.1
    #Only relevant if you make use of RESOLVCONF(8)
    #or similar...
  dns-nameservers 172.16.3.1


auto enp129s0f0
iface enp129s0f0 inet static
  address 172.16.3.2
  netmask 255.255.255.0
  broadcast 172.16.3.255
  gateway 172.16.3.1
    #Only relevant if you make use of RESOLVCONF(8)
    #or similar...
  dns-nameservers 172.16.3.1

auto enp6s0f0
iface enp6s0f0 inet dhcp


auto enp6s0f1
iface enp6s0f1 inet dhcp

```

This is only necessary for ubuntu 18.04

To enable the networking.

`systemctl unmask networking`

`systemctl enable networking`

`systemctl restart networking`


To chech the config.

`ifquery`

To bring up all the devices that is in the config.

`ifup -a`

Single interface.

`ifup yourinterface`

Take down the interface.

`ifdown yourinterface`

or panic mode 

`ifdown --all`




- Debian 

ifupdown, network-manager

- Centos 
networking-manager and for static can also via /etc/sysconfig/network-scripts/ifcfg-eth0 


