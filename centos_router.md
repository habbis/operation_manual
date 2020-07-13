
# Centos7/8 Router 

First set a host name either just edit ```/etc/hostanme``` manualy or ```hostnamectl```

```
vi /etc/hostname

hostnamectl set-hostname your-new-hostname

```

Then you can set up the interfaces you have two ways either manualy add to /etc/sysconfig/network-scripts or using ```nmtui/nmcli```
```
vi /etc/sysconfig/network-scripts/ifcfq-ens1

```
The uid is added when you create a interface in nmtui/nmcli also you can when manualy editing the public interface you can also set the zone in firewalld here in the config.

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


# IP forwarding

To allow ip forwarding create a new file unser ```/etc/sysctl.d/```

```
vi /etc/sysctl.d/ip_forward.conf
```
Add line.

```
net.ipv4.ip_forward=1
```
To enable ip forwarding.
```
sysctl -p /etc/sysctl.d/ip_forward.conf
```
# firewalld setup

Then to create a firewall rule to allow ip masquerading berween the public and private interface.

```
firewall-cmd --permanent --direct --passthrough ipv4 -t nat -I POSTROUTING -o eth0 -j MASQUERADE -s privat_ip/CIDR
```
Set the public interfae to the exsternal zone.

```
firewall-cmd --change-interface=ens1 --zone=external --permanent
```

Set the default zone to internal

```
firewall-cmd --set-default-zone=internal
```
To reload the firewall 
```
firewall-cmd --complete-reload
```
Then reboot and verify the firewall rules 
```
firewall-cmd --list-all 
firewall-cmd --list-all --zone=external
```
# DHCP-server

Then you also setup a dhcp server if you want like dnsmasq or isc-dhcp-server.

I am only going to show isc-dhcp-server.

Install it with yum.

```
# centos8
yum -y install dhcp-server

# centos7 
yum install -y dhcp
```
Next Choose the interface to have the dhcps-server on.
```
vi /etc/sysconfig/dhcpd
```
Add interface.
```
DHCPDARGS=ens1
```
THe location of the dhcp config file is ```/etc/dhcp/dhcpd.conf``` and its empty to find a example file just copy like this.
```
cp /usr/share/doc/dhcp-4.2.5/dhcpd.conf.example /etc/dhcp/dhcpd.conf 
```
Then start editing the file.
```
vi /etc/dhcp/dhcpd.conf 
```
Then change some options.
```
option domain-name "yourdomain";
option domain-name-servers 172.16.1.1, ns2.yournameserver;
deny declines;
default-lease-time 3600; 
max-lease-time 7200;
authoritative;

subnet 172.16.1.0 netmask 255.255.255.0 {
        option routers                  172.16.1.1;
        option subnet-mask              255.255.255.0;
        option domain-search            "yourdomain";
        option domain-name-servers      172.16.1.1;
        range   172.16.1.10   172.16.1.100;
        # if you wan more range like this
        range   172.16.1.120  172.16.1.200;
}

# assign static ip 

host ubuntu-node {
	 hardware  ethernet 00:f0:m4:6y:89:0g;
	 fixed-address 172.16.1.105;
 }

host fedora-node {
	 hardware  ethernet 00:4g:8h:13:8h:3a;
	 fixed-address 172.16.1.110;
 }
```

Then enable and start dhcp
```
systemctl start dhcpd
systemctl enable dhcpd
```

Then add a firewall rule to allow dhcp on internal interface
```
sudo firewall-cmd --zone=internal --add-service=dhcp --permanent
```

Link:

[Centos Router guide](https://ronnybull.com/2015/11/20/how-to-centos-7-router/)

[Change Hostname Centos](https://www.tecmint.com/set-change-hostname-in-centos-7/)

[DHCP server centos](https://www.tecmint.com/install-dhcp-server-in-centos-rhel-fedora/)
