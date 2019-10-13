# Fail2ban on centos7/8

First install ```epel-release``` then fail2ban.

```
yum install -y epel-release

yum install -y fail2ban
```

When using fail2ban you also have ```/etc/fail2ban/jail.conf``` with commeted out default that you can use.
To set up a fail2ban create the file ```/etc/fail2ban/jail.local```.

Add your config in this setup i will show sshd.

```
# THe global options
[DEFAULT]
#Ban hosts for one hour:
bantime = 1h
# THis is know good ip no need to worry
ignoreip = 10.16.1.0/24

# user firewalld as firewall for fail2ban 
banaction = firewallcmd-ipset
# spesific options for ssdd
[sshd]
enabled = true
#default port is 22
port = ssh
# logpath for this ban
logpath = %(sshd_log)s
```


Then when finished enable and start fail2ban.

```
systemctl enable fail2ban
systemctl start fail2ban
```




Link:

[Digital Ocean fail2ban centos](https://www.digitalocean.com/community/tutorials/how-to-protect-ssh-with-fail2ban-on-centos-7)

[Fedora wiki guide to fail2ban](https://fedoraproject.org/wiki/Fail2ban_with_FirewallD)
