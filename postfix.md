# Setup internal MTA (mail relay) server

I am using with [mxroute](https://mxroute.com/) email provider .

### Installing

debian/ubuntu

```
apt install -y postfix mailutils libsasl2-2 ca-certificates libsasl2-modules
```

postfix configuration wizard will ask you some questions. 
Just select your server as Internet Site and for FQDN use something like mta.example.com




Edit config file.
```
vim /etc/postfix/main.cf
```
Add what network can have access to the mta server.

```
mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128 192.168.1.0/24 

```

If you dont want to use ssl internaly comment out.

```
#smtp_tls_security_level=may
```

Add fqdn you want to use with the mta server.

```
mydestination = $myhostname,  hf-postfix01.local.net, localhost.local.net, localhost
```

Config for your email provider.

```
# your email provider smtp server google as example
relayhost = [smtp.gmail.com]:587
smtp_sasl_auth_enable = yes
# password for email provider
smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
smtp_sasl_security_options = noanonymous

```

Add password for username and password for email provider in this example google.


```
vim /etc/postfix/sasl_passwd
```

```
[smtp.gmail.com]:587    USERNAME@gmail.com:PASSWORD

```

Fix permission and update postfix config to use sasl_passwd file:

```
chmod 400 /etc/postfix/sasl_passwd
postmap /etc/postfix/sasl_passwd
```

Reload postfix.

```
/etc/init.d/postfix reload
```

Send test email.

```
echo "Test mail from postfix" | mail -s "Test Postfix" you@example.com
```

Wheck logs for problems.

```
tail -f /var/log/mail.log
```



Link: 

https://easyengine.io/tutorials/linux/ubuntu-postfix-gmail-smtp/
