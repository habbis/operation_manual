
# ldap auth apace 

centos 
```
dnf -y install mod_ldap                                                                                                                                                                                    
systemctl restart httpd                                                                                                                                                                                     
mkdir /var/www/html/auth 
```

```
chown www-data.www-data /var/www/html/auth -R                                                                                                                                                               
chown apache:apache /var/www/html/auth -R 
```
