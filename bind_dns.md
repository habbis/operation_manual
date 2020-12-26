# setup bind dns

BIND : Configure for Internal Network
2019/10/03
  	
Install BIND to Configure DNS (Domain Name System) Server to provide Name or Address Resolution service for Clients.



[1] 	Install BIND.
```
[root@dlp ~]# dnf -y install bind bind-utils
```

[2] 	On this example, Configure BIND for Internal Network.
The example follows is for the case that Local network is [10.0.0.0/24], Domain name is [srv.world], Replace them to your own environment.

```
[root@dlp ~]# vi /etc/named.conf


# add : set ACL entry for local network
acl internal-network {
        10.0.0.0/24;
};

options {
        # change ( listen all )
        listen-on port 53 { any; };
        # change if need ( if not listen IPv6, set [none] )
        listen-on-v6 { any; };
        directory       "/var/named";
        dump-file       "/var/named/data/cache_dump.db";
        statistics-file "/var/named/data/named_stats.txt";
        memstatistics-file "/var/named/data/named_mem_stats.txt";
        secroots-file   "/var/named/data/named.secroots";
        recursing-file  "/var/named/data/named.recursing";
        # add local network set on [acl] section above
        # network range you allow to recive queries from hosts
        allow-query     { localhost; internal-network; };
        # network range you allow to transfer zone files to clients
        # add secondary DNS servers if it exist
        allow-transfer  { localhost; };

        .....
        .....

        recursion yes;

        dnssec-enable yes;
        dnssec-validation yes;

        managed-keys-directory "/var/named/dynamic";

        pid-file "/run/named/named.pid";
        session-keyfile "/run/named/session.key";

        /* https://fedoraproject.org/wiki/Changes/CryptoPolicy */
        include "/etc/crypto-policies/back-ends/bind.config";
};

logging {
        channel default_debug {
                file "data/named.run";
                severity dynamic;
        };
};

zone "." IN {
        type hint;
        file "named.ca";
};

include "/etc/named.rfc1912.zones";
include "/etc/named.root.key";

# add zones for your network and domain name
zone "srv.world" IN {
        type master;
        file "srv.world.lan";
        allow-update { none; };
};
zone "0.0.10.in-addr.arpa" IN {
        type master;
        file "0.0.10.db";
        allow-update { none; };
};

# if you don't use IPv6 and also suppress logs for IPv6 related, possible to change

# set BIND to use only IPv4

[root@dlp ~]# vi /etc/sysconfig/named
# add to the end

OPTIONS="-4"

# For how to write the section [*.*.*.*.in-addr.arpa], write your network address reversely like follows
# case of 10.0.0.0/24
# network address        ⇒ 10.0.0.0
# network range          ⇒ 10.0.0.0 - 10.0.0.255
# how to write           ⇒ 0.0.10.in-addr.arpa

# case of 192.168.1.0/24
# network address        ⇒ 192.168.1.0
# network range          ⇒ 192.168.1.0 - 192.168.1.255
# how to write           ⇒ 1.168.192.in-addr.arpa
```
[3] 	

## BIND : Configure Zone Files
2019/10/03
  	
Configure Zone Files for each Zone set in [named.conf].
Replace Network or Domain name on the example below to your own environment.
[1] 	Create zone files that servers resolve IP address from Domain name.
The example below uses Internal network [10.0.0.0/24], Domain name [srv.world].
Replace to your own environment.

```
[root@dlp ~]# vi /var/named/srv.world.lan

$TTL 86400
@   IN  SOA     dlp.srv.world. root.srv.world. (
        # any numerical values are OK for serial number but
        # recommendation is [YYYYMMDDnn] (update date + number)
        2019100301  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
        # define Name Server
        IN  NS      dlp.srv.world.
        # define Name Server's IP address
        IN  A       10.0.0.30
        # define Mail Exchanger Server
        IN  MX 10   dlp.srv.world.

# define each IP address of a hostname
dlp     IN  A       10.0.0.30
www     IN  A       10.0.0.31

[2] 	Create zone files that servers resolve Domain name from IP address.
The example below uses Internal network [10.0.0.0/24], Domain name [srv.world].
Replace to your own environment.
[root@dlp ~]# vi /var/named/0.0.10.db

$TTL 86400
@   IN  SOA     dlp.srv.world. root.srv.world. (
        2019100301  ;Serial
        3600        ;Refresh
        1800        ;Retry
        604800      ;Expire
        86400       ;Minimum TTL
)
        # define Name Server
        IN  NS      dlp.srv.world.

# define each hostname of an IP address
30      IN  PTR     dlp.srv.world.
31      IN  PTR     www.srv.world.
```
[3] 	
Next, Start BIND and Verify Name or Address Resolution, refer to here. 




Links:

https://www.server-world.info/en/note?os=CentOS_8&p=dns&f=1