# SSL 




How to generate csr and private key for buying ssl cert.

```
openssl req -new -newkey rsa:2048 -nodes -keyout yourdomain.com.key -out yourdomain.com.csr
```
```
openssl req -new -newkey rsa:2048 -nodes -keyout osl.yourdomain.com.key -out osl.yourdomain.com.csr
```
```

Generating a RSA private key
..............................................................................................................................................+++++
...............................+++++
writing new private key to 'test.key'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]: US # your country code
State or Province Name (full name) [Some-State]: Texas # staste for your country
Locality Name (eg, city) []: Huston # your city
Organization Name (eg, company) [Internet Widgits Pty Ltd]: nocompany # your company if you need it 
Organizational Unit Name (eg, section) []: washer # org in the company
Common Name (e.g. server FQDN or YOUR name) []: mysite.yourdomain.com or if its wildcard *.yourdomain.com
Email Address []: yourmail@yourdomain.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []: # you can user it if your wamt
An optional company name []:  # you can user it if your wamt


```
