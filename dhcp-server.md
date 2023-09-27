# Setup dhcp server Linux



### Installing 

Debian


To Install all of kea dhcp server with dhcp4 and dhcp6 server.
```
sudo apt install -y  kea
```

If you want only dhcp4-server for ipv4
```
sudo apt install -y kea-dhcp4-server
```

If you want only dhcp6-server for ipv6
```
sudo apt install -y kea-dhcp4-server
```

### Setup dhcp4 config file
```
vim /etc/kea/kea-dhcp4.conf
```

Here is a example file with storage backed memfile and it will store dhcp lease in csv.

Remember to to change interface eth0 to your own.

```
{
"Dhcp4": {
    "interfaces-config": {
        "interfaces": ["eth0"]
    },

    "lease-database": {
        "type": "memfile",
        "persist": true,
        "name": "/var/lib/kea/kea-leases4.csv",
        "lfc-interval": 3600
    },

    "renew-timer": 15840,
    "rebind-timer": 27720,
    "valid-lifetime": 31680,

    "option-data": [
        {
            "name": "domain-name-servers",
            "data": "1.1.1.1, 8.8.8.8"
        },

        {
            "name": "domain-search",
            "data": "local.net"
        }
    ],

    "subnet4": [
        {
            "subnet": "192.168.1.0/24",
            "pools": [ { "pool": "192.168.1.50 - 192.168.1.100" } ],
            "option-data": [
                {
                    "name": "routers",
                    "data": "192.168.1.1"
                }
            ]

        }

    ]
}
}
```




## Kea dhcp doc 

[kea doc](https://kea.readthedocs.io/en/latest/arm/intro.html)



