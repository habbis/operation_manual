To install freeipa server on centos 7 
```
yum install ipa-server
```

If you want dns with your freeipa

```
yum install -y ipa-server-dns

```

Then to setup the freeipa server run 

```
ipa-server-install
```

Then to have zone overlap run it like this 
```
ipa-server-install --allow-zone-overlap
```

To setup a client to connect the freeipa 

```
yum install -y ipa-client
```
Then to run the setup 
```
ipa-client-install --server server.example.com --domain example.com
```
After the installation, authenticate to the Kerberos realm to ensure that the administrator is properly configured.
```
kinit admin
```
You can also print basic account information to verify that the SSSD service is running as expected:
```
id admin
```
Here is the command to open the firewall for freeipa 

```
firewall-cmd --zone=public --add-service=freeipa-ldap --add-service=freeipa-ldaps --add-service=freeipa-replication --add-service=ntp --add-service=  --add-service=http --add-service=https --permanent

```

# setup freeip server rhel8/oralce linux 8 

enable ipa-server module 

```
yum module enable idm:DL1
```
Then install ipa-server 
```
yum module install idm:DL1/dns
```


```

Disable ipv6
```
vim /etc/sysctl.conf
```
Then to paste in
```
net.ipv6.conf.all.disable_ipv6 = 0
```
Then to enable the setting
```
sysctl -p
```


## Testing LetsEncrypt setup with freeipa

First download the CA certificate to get the trust [DTSRoot CA certfile](https://www.identrust.com/certificates/trustid/root-download-x3.html) and download the ca from [Let's Encrypt certfile](https://letsencrypt.org/certificates/) .

Here is example files 

I did download [isrg-root-x1-cross-signed.pem](https://letsencrypt.org/certs/isrg-root-x1-cross-signed.pem) and [dst-root-ca-x3](https://www.identrust.com/dst-root-ca-x3)

when downloading this cert [DTSRoot CA certfile](https://www.identrust.com/certificates/trustid/root-download-x3.html) paste it into a file called
`DTSRootCAX3.pem` so its easier to handle.

[isrg-root-x1-cross-signed.pem](https://letsencrypt.org/certs/isrg-root-x1-cross-signed.pem) is alerdy ready to use. 


setup certboot or other tools to download letsencrypt certs i use acme package with pfsense and the cloudflare api so sign certs.

transfere all the cert to the ipa server i am placing the certs on `/etc/ssl/`

Then import the ca cert into freeipa.
```
ipa-cacert-manage -n DTSRootCAX3 -t C,, install DTSRootCAX3.pem
ipa-cacert-manage -n LetsEncryptX3 -t C,, install isrg-root-x1-cross-signed.pem
```

Then add the DTSRootCAX3.pem , isrg-root-x1-cross-signed.pem into a file called yourdomain.all.pem this file have both crt and key and chain. 
Since i use acem with pfsense i do get that file and it should come with certbot. 

```
cat DTSRootCAX3.pem >> yourdomain.all.pem
```
```
cat isrg-root-x1-cross-signed.pem >> yourdomain.all.pem
```
Install the cert into freeipa.
```
ipa-server-certinstall -w /etc/ssl/yourdomain.all.pem
```

Then stop and start freeipa services.
```
ipactl stop
ipactl start
```

