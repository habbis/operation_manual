

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

Verbose Output

To display additional information such as logging level, default policies, and new profiles, use status verbose:
Terminal
```
sudo ufw status verbose

output

Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
```
Numbered Output

Use status numbered to display the order and ID number of each rule. This is useful when you need to delete a specific rule by its number:
Terminal
```
sudo ufw status numbered

output

Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 80/tcp                     ALLOW IN    Anywhere
[ 3] 443/tcp                    ALLOW IN    Anywhere
[ 4] 8069/tcp                   ALLOW IN    Anywhere
```
Deleting UFW Rules

There are two ways to delete UFW rules:

    By rule number: Easier when you have many rules. List the numbered rules and specify which number to delete.
    By specification: Specify the full rule definition to remove it.

Warning
If you are managing the firewall over SSH, do not remove the rule that allows SSH traffic (port 22 by default). Deleting it will lock you out of the server.
Delete by Rule Number

First, list the rules with their numbers:
Terminal
```
sudo ufw status numbered

output

Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 80/tcp                     ALLOW IN    Anywhere
[ 3] 443/tcp                    ALLOW IN    Anywhere
[ 4] 8069/tcp                   ALLOW IN    Anywhere
```
To delete rule number 4 (port 8069), run:
Terminal
```
sudo ufw delete 4

UFW asks for confirmation before deleting:
output

Deleting:
 allow 8069/tcp
Proceed with operation (y|n)? y
Rule deleted
```
Type y and press Enter to confirm. Each time you remove a rule, the remaining rule numbers shift. Always list the rules again before deleting another one.

If you need a non-interactive deletion (for scripts), use:
Terminal
```
sudo ufw --force delete 4

Delete by Specification
```
You can also delete a rule by specifying its full definition. This method does not require listing numbered rules first.

For example, if you previously added a rule to allow port 2222:
Terminal
```
sudo ufw allow 2222
```
You can delete it by repeating the rule after ufw delete:
Terminal
```
sudo ufw delete allow 2222
```
This also works with more specific rules. To delete a rule that allows TCP traffic on port 80 from a specific subnet:
Terminal
```
sudo ufw delete allow from 192.168.1.0/24 to any port 80 proto tcp
```
Reset UFW and Remove All Rules

Resetting UFW disables the firewall and removes all active rules. This is useful when you want to revert all changes and start with a clean configuration:
Terminal
```
sudo ufw reset
```
UFW creates backup files of the current rules before resetting. The backup file paths are displayed in the output.

Link:

https://www.configserverfirewall.com/ufw-ubuntu-firewall/ufw-allow-ip-address-ubuntu-firewall/
https://linuxize.com/post/how-to-list-and-delete-ufw-firewall-rules/
