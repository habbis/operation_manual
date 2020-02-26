
# Networking for servers


## Linux networking 

Manage Linux networking.


- Ubuntu, Debian 

ifupdown(via networking-manager), netplan(ubuntu), network-manager

Config setup

ifupdown

sudo apt install -y ifupdown network-manager

`vim /etc/NetworkManager/NetworkManager.conf`
Will look like this

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


It will look like this and remember to add auto if you dont the inerface will not 
enable networking at boot.

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


- Centos 
networking-manager and for static can also via /etc/sysconfig/network-scripts/ifcfg-eth0 
First set a host name either just edit ```/etc/hostanme``` manualy or ```hostnamectl```

```
vi /etc/hostname

hostnamectl set-hostname your-new-hostname

```

Then you can set up the interfaces you have two ways either manualy add to /etc/sysconfig/network-scripts or using ```nmtui/nmcli```
```
vi /etc/sysconfig/network-scripts/ifcfq-ens1

```
The uid is added when you create a interface in nmtui/nmcli.

```
BOOTPROTO=static
DEVICE=ens3
ONBOOT=yes
IPADDR="<PUBLIC_IP>"
NETMASK="<PUBLIC_NETMASK>"
GATEWAY="<PUBLIC_DEFAULT_GATEWAY>"
DNS1="<PUBLIC_DNS>
NAME="System ens3"
ZONE=external`

```
dhcp config.
```
TYPE=Ethernet
DEVICE=ens3
ONBOOT=yes
BOOTPROTO=dhcp
IPV6INIT=yes
IPV6_AUTOCONF=yes
PROXY_METHOD=none
BROWSER_ONLY=no
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
NAME="System ens3"
UUID=21d47e65-8523-1a06-af22-6f121086f085
ZONE=external

```
 The LAN interface

static config

```
BOOTPROTO=static
ONBOOT=yes
IPADDR="<PRIVATE_IP>"
NETMASK="<PRIVATE_NETMASK>"
DNS1="<PUBLIC_DNS>"

```
dhcp config 
```
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=none
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=no
NAME="system ens7"
UUID=1cd0b2cc-2d47-4dfb-9eb1-7a45895e90c0
ONBOOT=yes
IPADDR=10.16.1.1
PREFIX=24
GATEWAY=10.16.1.1
DEVICE=ens7
DNS1=1.1.1.1
ZONE=internal
```
If you user network manager you can user ```nmcli`` to start up ther interface after the config setup.
```
nmcli con load /etc/sysconfig/network-scripts/ifcfg-ens1
nmcli con up 'System ens1'
```


