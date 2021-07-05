
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

config file example with active directory 

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
    AuthLDAPBindPassword yourbinduserpass
    Require ldap-group CN=yourgroup,OU=groups,DC=intern,DC=local,DC=net
    Require valid-user
</Directory>

```
freeipa example with openldap
```
<Directory "/var/www/html/auth">
#<Directory "/">
    #SSLRequireSSL
    Options Indexes FollowSymlinks
    AuthType Basic
    AuthName "LDAP Authentication"
    #AuthBasicAuthoritative Off
    AuthBasicProvider ldap
    AuthLDAPURL "ldap://ipa01:389/cn=users,dc=ipa,dc=local,dc=net?inetuser?sub?(objectClass=*)"
    AuthLDAPBindDN nginx-bind@ipa.local.net
    AuthLDAPBindPassword yourbinduserpass
    Require ldap-group CN=yourgroup,OU=groups,dc=ipa,dc=local,dc=net
    Require valid-user
</Directory>

```

How you can use this with nginx reverse proxy 

```
  ssl_certificate /config/local.net.crt.pem;
  ssl_certificate_key /config/local.net.key.pem;
  ssl_session_timeout  5m;
  access_log /var/log/nginx/access.log;
  error_log /var/log/nginx/error.log;

server {
  listen 80; #default_server;
  listen 443 ssl;
  server_name *.local.net;
  return 301 https://$host$request_uri;
  #rewrite ^ https://$host$request_uri? permanent;
  #ssl on;


  #if ($http_x_forwarded_proto != 'https'){
  #    return 301 https://$server_name$request_uri;
  #}

}


server {
  listen 443 ssl;
  server_name yourhost.local.net;
  #ssl on;
  location / {
    auth_request /auth;
    ##try_files $uri $uri/ =404;
    proxy_pass http://yourserver.intern.local.net/a:9000;

  location = /auth {
           proxy_pass http://ldapauth.intern.local.net/auth/;
           proxy_pass_request_body off;
           proxy_set_header Content-Length "";
           proxy_set_header X-Original-URI $request_uri;
   }
  }


```
