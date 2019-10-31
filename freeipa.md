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