To sync time 
```
 w32tm /config /syncfromflags:manual /manualpeerlist:"0.pool.ntp.org 1.pool.ntp.org 2.pool.ntp.org 3.pool.ntp.org"
 
 w32tm /config /update
 
 w32tm /resync
 ```
 
