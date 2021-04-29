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

get property from regedit 
```
Get-ItemProperty HKLM:\your/path
```


Having the error "Verification of outbound replication failed" when adding a new DC?

So I had plans to create some redundancy for my domain by adding a slave domain controller. The primary domain controller was however somewhat obnoxious and returned an error when I tried to let my new domain controller join it.

    "Verification of outbound replication failed. Outbound replication is not enabled on replication source domain controller: dc01.corp.com"

The setup
Primary domain controller

    dc01.corp.com
    Windows 2012 Server (standard)

Secondary domain controller

    dc02.corp.com
    Windows 2012 Server (standard)

To solve my issues I had to rely on the good old repadmin, http://technet.microsoft.com/en-us/library/cc770963(v=WS.10).aspx

Begin by checking the current options
```
repadmin /options dc01.corp.com
Current DSA Options: IS_GC DISABLE_INBOUND_REPL DISABLE_OUTBOUND_REPL
```
Aha! DISABLE_OUTBOUND_REPL, this shouldn't be there. Remove it like this
```
repadmin /options dc01.corp.com -DISABLE_OUTBOUND_REPL
Current DSA Options: IS_GC DISABLE_INBOUND_REPL DISABLE_OUTBOUND_REPL
New DSA Options: IS_GC DISABLE_INBOUND_REPL
```
There, all good. Just go again and hopefully you'll do fine. Or get another error. :) 
