
# ldap auth apace 

centos 
```
dnf -y install mod_ldap                                                                                                                                                                                    
systemctl restart httpd                                                                                                                                                                                     
mkdir /var/www/html/auth 
```
permissions 
```
chown www-data.www-data /var/www/html/auth -R                                                                                                                                                               
chown apache:apache /var/www/html/auth -R 
```

setup config file
```
vim /etc/httpd/conf.d/authnz_ldap.conf  
```

config file 

```
<Directory "/var/www/html/auth">
#<Directory "/">
    #SSLRequireSSL
    Options Indexes FollowSymlinks
    AuthType Basic
    AuthName "LDAP Authentication"
    #AuthBasicAuthoritative Off
    AuthBasicProvider ldap
    AuthLDAPURL "ldap://yourhostname/CN=Users,DC=intern,DC=local,DC=net?sAMAccountName?sub?(objectClass=*)"
    AuthLDAPBindDN nginx-bind@intern.local.net
    AuthLDAPBindPassword yourbinduseroass
    Require ldap-group CN=yourgroup,OU=groups,DC=intern,DC=local,DC=net
    Require valid-user
</Directory>

```
