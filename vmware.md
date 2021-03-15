
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
If you get ssl error you can disable checking 
```
Set-PowerCLIConfiguration -InvalidCertificateAction:Ignore
```

esxi cli

```
esxcli storage nfs41 add -H 10.18.1.10 -s /mnt/larsen/vmstorage02 -v vmstorage02
esxcli storage nfs41 add -H 10.18.1.10 -s /mnt/larsen/vca02 -v vca02


esxcli storage nfs41 list

esxcli storage nfs41 remove -v yourdatastore

esxcli core

esxicli software vib

```


powercli 

```
Install-Module VMware.PowerCLI


Set-PowerCLIConfiguration -InvalidCertificateAction:ignore


Set-PowerCLIConfiguration -Scope User -ParticipateInCEIP $false



new-datastore -Nfs -Name vmstorage02 -path "/mnt/larsen/vmstorage02" -NfsHost 10.18.1.10




To disable this warning and set your preference use the following command and restart PowerShell: 
Set-PowerCLIConfiguration -Scope User -ParticipateInCEIP $true or $false.

```



[powercli-doc](https://code.vmware.com/docs/11860/powercli-12-0-0-user-s-guide/GUID-FDB2591C-121E-40F9-AD83-317E7DF9B704.html)
