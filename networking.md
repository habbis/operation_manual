
# Networking for servers


## Linux networking 

- Ubuntu 
ifupdown(via networking-manager), netplan, network-manager

ifupdown

sudo apt install -y ifupdown network-manager

vim /etc/NetworkManager/NetworkManager.conf 

It will look like this 

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




- Debian 

ifupdown, network-manager

- Centos 
networking-manager and for static can also via /etc/sysconfig/network-scripts/ifcfg-eth0 


