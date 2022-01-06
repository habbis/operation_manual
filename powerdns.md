# setup powerdns authoritative and powerdns recursor


Powerdns repos for both authoritative and recursor:

https://repo.powerdns.com/


So you have to choose between distro maintained packages or repo from the vendor 
and there is pros and cons to both.


First you must choose your backend on powerdns authoritative server.

You have

- ldap
- lua2
- mysql/mariadb
- postgres
- remote
- sqlite
- tinydns


In this guide we will focus on mysql and postgresql see powerdns doc for more options.

Installing the packages

debian/ubuntu

```
```
rhel 8 based - you need epel first

```
dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
```

