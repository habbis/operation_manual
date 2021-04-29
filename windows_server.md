# windows server

## To sync time 
```
 w32tm /config /syncfromflags:manual /manualpeerlist:"0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org"
 
 w32tm /config /update
 
 w32tm /resync
 ```
 
## Remove and add window key

update key when running evaluation
```
dism /online /set-edition:ServerStandard /productkey:yourkey /accepteula
```

remove cd key
```
slmgr.vbs -upk
```

update cd key
```
slmgr.vbs -upk yourkey
```

Get som info like what edition you are running
```
DISM /Online /Get-CurrentEdition

```

install AD via powerhell
```
Install-WindowsFeature -name AD-Domain-Services -IncludeManagementTools
```
