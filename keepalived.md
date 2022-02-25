# setup keepalived


## install keepalived and killall utils.

rhel based.
```
yum install -y keepalived psmisc
```


debian based.
```
apt install -y  keepalived psmisc
```

Then to setup a bacis config with master ans backup server 
with a virtual ip that will transfere when the main server goes down.

The vip should be on the same ip ranger as the servers you are settting up.


```
vim /etc/keepalived/keepalived.conf
```

Master config.

vvrrp_scrip chk_wg0 is using killall -0 to check if process is running of not failover to backup.

If you you are using somthing else just use the proccess name of your program like haproxy or nginx.
test with pgrep to find is the name is right.

```
pgrep haproxy
```





```
# Define the script used to check if wireguard is still working
vrrp_script chk_wg0 {
    # This will check if the wireguard pid of tunnel wg0 is install running if no then failover to the backup.
    script "/usr/bin/killall -0 wg-crypt-wg0"
    interval 2
    weight 2
}

# Configuration for Virtual Interface
vrrp_instance LB_VIP {
    # choose the your interface
    interface eth0
    state MASTER        # set to BACKUP on the peer machine
    priority 101        # set to  99 on the peer machine
    virtual_router_id 51

 
    # No need to allow ssh for root user it will failover
    authentication {
        auth_type PASS
        # pass only for keepalived
        auth_pass yourpass    # Password for accessing vrrpd. Same on all devices
    }
    unicast_src_ip 192.168.4 # Private IP address of master
    unicast_peer {
        192.168.26.5              # Private IP address of the backup wireguard
   }

    # The virtual ip address shared between the two loadbalancers
    virtual_ipaddress {
        192.168.26.21
    }

    # Use the Defined Script to Check whether to initiate a fail over
    track_script {
        chk_wg0
    }


```

Then enable and start service may only be needed on rhel based distro.
```
systemctl enable --now keepalived.service
```


Backup config.

```
vim /etc/keepalived/keepalived.conf 

```

```
vrrp_script chk_wg0 { 
    script "/usr/bin/killall -0 wg-crypt-wg0"
    interval 2 
    weight 2 
}
  
vrrp_instance LB_VIP {
    # choose the your interface
    interface eth0
    state BACKUP
    priority 100
    virtual_router_id 51
  
  
    authentication {
        auth_type PASS
        auth_pass yourpass
    }
    unicast_src_ip 192.168.26.5 # Private IP address of the backup wireguard
    unicast_peer {
        192.168.26.4      # Private IP address of the master wireguard
   }
  
    virtual_ipaddress {
        192.168.26.21
    }
     
    track_script {
        chk_wg0
    }
}

```










