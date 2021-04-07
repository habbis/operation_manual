# proxmox tips and tricks 

to add nfs storage via cli 
```
pvesm add nfs pvc_vmstorage01 --path /mnt/pve/pvc_vmstorage01 --server 10.18.1.11 --export /mnt/larsen/pvc_vmstorage01 --options vers=4.1

pvesm add nfs vmstorage02 --path /mnt/pve/vmstorage02 --server 10.18.1.11 --export /mnt/larsen/vmstorage02 --options vers=4.1

pvesm add nfs vmstorage_service --path /mnt/pve/vmstorage_service --server 10.18.1.11 --export /mnt/larsen/vmstorage_service --options vers=4.1

pvesm add nfs iso --path /mnt/pve/iso --server 10.18.1.11 --export /mnt/larsen/iso --options vers=4.1
```

To remove the invalid subscription
```
sed -Ezi.bak "s/(Ext.Msg.show\(\{\s+title: gettext\('No valid sub)/void\(\{ \/\/\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js && systemctl restart pveproxy.service 
```
To convert a vmwaer disk to raw og qcow2 and copy all vmdk files.
```
qemu-img convert -f vmdk unifi01.vmdk -O qcow2 unifi01.qcow2
```


To setup vlan and bridges you can edit /etc/network/interface and on proxmox bridge must be named vmbr*

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

iface eno2 inet manual

iface eno2.22 inet manual

iface eno1 inet manual

iface ens1.6 inet manual
        mtu 9000

iface ens1.7 inet manual
        mtu 9000


# The primary network interface
allow-hotplug ens1
iface ens1 inet manual
      mtu 9000
#       address 10.18.1.10/24
#       gateway 10.18.1.1
#       # dns-* options are implemented by the resolvconf package, if installed
#       dns-nameservers 10.18.1.1
#       dns-search no.habbfarm.net
#
#

auto vmbr0
iface vmbr0 inet static
        address 10.21.1.5/24
        gateway 10.21.1.1
        bridge-ports ens1.6
        bridge-stp off
        bridge-fd 0
        mtu 9000
#iface vmbr0 inet dhcp

auto vmbr1
iface vmbr1 inet static
        address 10.20.1.5/24
        bridge-ports ens1.7
        bridge-stp off
        bridge-fd 0
        mtu 9000

auto vmbr2
iface vmbr2 inet static
        address 10.18.1.5/24
        gateway 10.18.1.1
        bridge-ports ens1
        bridge-stp off
        bridge-fd 0
        mtu 9000

auto vmbr3
iface vmbr3 inet static
        #address 10.1.1.9/24
        #gateway 10.1.1.1
        bridge-ports eno2
        bridge-stp off
        bridge-fd 0

```





links:

https://www.cyberciti.biz/faq/perl-warning-setting-locale-failed-in-debian-ubuntu/
