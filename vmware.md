
# vmware notes 


## vcenter

Command line: 

on venter 6.* to list all services stopped or started 

```
service-control --status --all 
```

To setup or regenerate sertificates on vcenter 6.* run this 

```
 /usr/lib/vmware-vmca/bin/certificate-manager
```


## Powercli

To connect to center with powercli

```
Connect-VIServer -Server 10.23.112.235 -Protocol https -Username 'Adminis!ra!or' -Password 'pa$$word'
```

[powercli-doc](https://code.vmware.com/docs/11860/powercli-12-0-0-user-s-guide/GUID-FDB2591C-121E-40F9-AD83-317E7DF9B704.html)
