

# ufw firewall


Allow ssh connection.
```
ufw allow ssh 
```
Block incoming connection.
```
ufw default deny  incoming
```
Allow outging so you can connect to internet.
```
ufw default allow outgoing
```
To enable ufw
```
ufw enable
```

How to Allow IP Address in Ubuntu Firewall

In This UFW Tutorial we are going to learn how to allow IP Address from Ubuntu Firewall.

In Ubuntu Firewall we can add rules to allow IP Address to All Traffic or for certain network ports using ufw allow command.
```
ufw allow from <Remote-IP> to <local-IP>
```
Examples
```
ufw allow from 192.168.1.50
```
This Firewall rule will allow all traffic from the IP Address 192.168.1.50.
How to Allow IP Address in Ubuntu Firewall
  
```
ufw allow from 192.168.1.50 to 192.168.0.10
```
  
Allow all traffic from IP 192.168.1.50, But only on Local Server IP 192.168.0.10.
```
ufw allow from 192.168.1.10 to any proto tcp
```
This time we allow all network traffic related to the TCP protocol to the IP Address 192.168.1.10 from the Ubuntu firewall.
```
ufw allow from 192.168.1.10 to any proto tcp port 80
```
Open Port 80 (HTTP Traffic) to the IP Address 192.168.1.10 from Ubuntu Firewall.
Allow IP Address port 80 in Ubuntu Firewall
```
ufw allow from 192.168.1.10 to any proto udp port 53
```
  
This Rule will Open UDP port 53 to IP 192.168.1.10.
Allow IP Network From Ubuntu Firewall

Using subnet mask prefix we can allow entire subnetwork from the UFW Firewall.
```
ufw allow from 192.168.1.0/24 to any proto tcp port 21
```
  
This Rule will Allow FTP Traffic on 192.168.1.0/24 Netwok.
Summary

In This Tutorial We learned How to Allow IP From the UFW Firewall using ufw allow Command. As you learned, we can allow IP Address for All network traffic or to certain network ports.

Link:

https://www.configserverfirewall.com/ufw-ubuntu-firewall/ufw-allow-ip-address-ubuntu-firewall/
