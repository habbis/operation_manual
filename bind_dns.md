# setup bind dns

BIND : Configure for Internal Network
2019/10/03
  	
Install BIND to Configure DNS (Domain Name System) Server to provide Name or Address Resolution service for Clients.
[1] 	Install BIND.
[root@dlp ~]# dnf -y install bind bind-utils
[2] 	On this example, Configure BIND for Internal Network.
The example follows is for the case that Local network is [10.0.0.0/24], Domain name is [srv.world], Replace them to your own environment.
[root@dlp ~]# vi /etc/named.conf

```
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
Next, Configure Zone Files for each Zone you set in [named.conf] above.
To Configure Zone Files, refer to here. 