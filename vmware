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
