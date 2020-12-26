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

BIND : Verify Resolution
2019/10/03
[1] 	Start and Enable BIND.
```
[root@dlp ~]# systemctl enable --now named
```
[2] 	If Firewalld is running, allow DNS service. DNS uses [53/TCP,UDP].
```
[root@dlp ~]# firewall-cmd --add-service=dns --permanent

success

[root@dlp ~]# firewall-cmd --reload

success
```

[3] 	Change DNS setting to refer to own DNS if need.
(replace [ens2] to your own environment).
```
[root@dlp ~]# nmcli connection modify ens2 ipv4.dns 10.0.0.30

[root@dlp ~]# nmcli connection down ens2; nmcli connection up ens2
```

[4] 	Verify Name and Address Resolution. If [ANSWER SECTION] is shown, that's OK.

```
[root@dlp ~]# dig dlp.srv.world.


; <<>> DiG 9.11.4-P2-RedHat-9.11.4-17.P2.el8_0.1 <<>> dlp.srv.world.
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 4141
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: 0e9ca06c44ffe5e20209ad655d954316681848d8675423ab (good)
;; QUESTION SECTION:
;dlp.srv.world.                 IN      A

;; ANSWER SECTION:
dlp.srv.world.          86400   IN      A       10.0.0.30

;; AUTHORITY SECTION:
srv.world.              86400   IN      NS      dlp.srv.world.

;; Query time: 0 msec
;; SERVER: 10.0.0.30#53(10.0.0.30)
;; WHEN: Thu Oct 02 19:38:46 JST 2019
;; MSG SIZE  rcvd: 100

[root@dlp ~]# dig -x 10.0.0.30


; <<>> DiG 9.11.4-P2-RedHat-9.11.4-17.P2.el8_0.1 <<>> -x 10.0.0.30
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 61063
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 1, ADDITIONAL: 2

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
; COOKIE: cf5c64acd453263666c674db5d9543400d47bea92e41b7e1 (good)
;; QUESTION SECTION:
;30.0.0.10.in-addr.arpa.                IN      PTR

;; ANSWER SECTION:
30.0.0.10.in-addr.arpa. 86400   IN      PTR     dlp.srv.world.

;; AUTHORITY SECTION:
0.0.10.in-addr.arpa.    86400   IN      NS      dlp.srv.world.

;; ADDITIONAL SECTION:
dlp.srv.world.          86400   IN      A       10.0.0.30

;; Query time: 0 msec
;; SERVER: 10.0.0.30#53(10.0.0.30)
;; WHEN: Thu Oct 02 19:39:28 JST 2019
;; MSG SIZE  rcvd: 136

```

Links:

https://www.server-world.info/en/note?os=CentOS_8&p=dns&f=1
https://www.server-world.info/en/note?os=CentOS_8&p=dns&f=3
https://www.server-world.info/en/note?os=CentOS_8&p=dns&f=4
