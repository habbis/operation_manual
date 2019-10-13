# Firewalld Guide


Start firewalld

```
sudo systemctl start firewalld
sudo systemctl enable firewalld
```

Check status.

```
firewall-cmd --state
```
Firewalld daemon state
```
systemclt stastus firewalld
```
To reload firewalld.
```
firewall-cmd --relaod
```
# Configuring Firewalld
```/usr/lib/FirewallD``` holds default configurations like default zones and common services. 
Avoid updating them because those files will be overwritten by each firewalld package update.
```/etc/firewalld holds``` system configuration files. These files will overwrite a default configuration.

# Configuration Sets 

Firewalld uses two configuration sets: Runtime and Permanent. 
Runtime configuration changes are not retained on reboot or upon restarting 
FirewallD whereas permanent changes are not applied to a running system.

By default, ```firewall-cmd``` commands apply to runtime configuration but using the ```--permanent``` flag will establish a persistent configuration. To add and activate a permanent rule, you can use one of two methods.

Add the rule to both the permanent and runtime sets.
   
```
 firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --zone=public --add-service=http
```
Add the rule to the permanent set and reload FirewallD.

```
firewall-cmd --zone=public --add-service=http --permanent
firewall-cmd --reload
```


# Firewall Zones

Zones are pre-constructed rulesets for various trust levels you would likely have for a given location or scenario 
(e.g. home, public, trusted, etc.). 
Different zones allow different network services and incoming traffic types while denying everything else. 
After enabling FirewallD for the first time, Public will be the default zone.

Zones can also be applied to different network interfaces. 
For example, with separate interfaces for both an internal network and the Internet, 
you can allow DHCP on an internal zone but only HTTP and SSH on external zone. 
Any interface not explicitly set to a specific zone will be attached to the default zone.

To view the default zone:

To view the default zone:
```
firewall-cmd --get-default-zone
```
To change the default zone:
```
 firewall-cmd --set-default-zone=internal
```
To see the zones used by your network interface(s):
```
 firewall-cmd --get-active-zones
```
To get all configurations for a specific zone:
```
 firewall-cmd --zone=public --list-all
```

# Working with Services

FirewallD can allow traffic based on predefined rules for specific network services. You can create your own custom service rules and add them to any zone. 
The configuration files for the default supported services are located at ```/usr/lib/firewalld/services``` and user-created service files would be in ```/etc/firewalld/services```.

To view the default available services:
```
sudo firewall-cmd --get-services
```
As an example, to enable or disable the HTTP service:
```
sudo firewall-cmd --zone=public --add-service=http --permanent
sudo firewall-cmd --zone=public --remove-service=http --permanent
```

# Allowing or Denying an Arbitrary Port/Protocol

As an example: Allow or disable TCP traffic on port 12345.

sudo firewall-cmd --zone=public --add-port=12345/tcp --permanent
sudo firewall-cmd --zone=public --remove-port=12345/tcp --permanent

# Port Forwarding

The example rule below forwards traffic from port 80 to port 12345 on the same server.

sudo firewall-cmd --zone="public" --add-forward-port=port=80:proto=tcp:toport=12345

To forward a port to a different server:

    Activate masquerade in the desired zone.
```
  firewall-cmd --zone=public --add-masquerade
```
    Add the forward rule. This example forwards traffic from local port 80 to port 8080 on a remote server located at the IP address: 198.51.100.0.
```
 firewall-cmd --zone="public" --add-forward-port=port=80:proto=tcp:toport=8080:toaddr=198.51.100.0
```
To remove the rules, substitute --add with --remove. For example:
```
 firewall-cmd --zone=public --remove-masquerade
```

$ Constructing a Ruleset with FirewallD

As an example, here is how you would use FirewallD to assign basic rules to your Linode if you were running a web server.

    Assign the dmz zone as the default zone to eth0. Of the default zones offered, dmz (demilitarized zone) is the most desirable to start with for this application because it allows only SSH and ICMP.
```
 firewall-cmd --set-default-zone=dmz
 firewall-cmd --zone=dmz --add-interface=eth0
```
    Add permanent service rules for HTTP and HTTPS to the dmz zone:
```
 firewall-cmd --zone=dmz --add-service=http --permanent
 firewall-cmd --zone=dmz --add-service=https --permanent
```
    Reload FirewallD so the rules take effect immediately:
```
  firewall-cmd --reload
```
    If you now run firewall-cmd --zone=dmz --list-all, this should be the output:
```
    dmz (default)
      interfaces: eth0
      sources:
      services: http https ssh
      ports:
      masquerade: no
      forward-ports:
      icmp-blocks:
      rich rules:
```
    This tells us that the dmz zone is our default which applies to the eth0 interface, all network sources and ports. Incoming HTTP (port 80), HTTPS (port 443) and SSH (port 22) traffic is allowed and since there are no restrictions on IP versioning, this will apply to both IPv4 and IPv6. Masquerading and port forwarding are not allowed. We have no ICMP blocks, so ICMP traffic is fully allowed, and no rich rules. All outgoing traffic is allowed.

# Rich Rules

Rich rules syntax is extensive but fully documented in the firewalld.richlanguage(5) man page (or see man firewalld.richlanguage in your terminal). Use --add-rich-rule, --list-rich-rules and --remove-rich-rule with firewall-cmd command to manage them.

Here are some common examples:

Allow all IPv4 traffic from host 192.0.2.0.
```
sudo firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address=192.0.2.0 accept'
```
Deny IPv4 traffic over TCP from host 192.0.2.0 to port 22.
```
sudo firewall-cmd --zone=public --add-rich-rule 'rule family="ipv4" source address="192.0.2.0" port port=22 protocol=tcp reject'
```
Allow IPv4 traffic over TCP from host 192.0.2.0 to port 80, and forward it locally to port 6532.
```
sudo firewall-cmd --zone=public --add-rich-rule 'rule family=ipv4 source address=192.0.2.0 forward-port port=80 protocol=tcp to-port=6532'
```
Forward all IPv4 traffic on port 80 to port 8080 on host 198.51.100.0 (masquerade should be active on the zone).
```
sudo firewall-cmd --zone=public --add-rich-rule 'rule family=ipv4 forward-port port=80 protocol=tcp to-port=8080 to-addr=198.51.100.0'
```
To list your current Rich Rules in the public zone:
```
sudo firewall-cmd --zone=public --list-rich-rules
```
iptables Direct InterfacePermalink

For the most advanced usage, or for iptables experts, FirewallD provides a direct interface that allows you to pass raw iptables commands to it. Direct Interface rules are not persistent unless the --permanent is used.

To see all custom chains or rules added to FirewallD:
```
firewall-cmd --direct --get-all-chains
firewall-cmd --direct --get-all-rules

```

