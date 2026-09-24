# openbsd tips 


I am running openbsd on Norwegian vpns provider https://shrp.no/en/


They are using convoy to manage vps since they are using serial terminal the openbsd will place you into `boot>` promt to get the installert to work run this.
```
boot> set tty com0
```

Then boot into the installer
```
boot> boot
```

Change shell to bash for user.

First install bash
```
pkg_add -v bash
```
Change shell for root 
```
chsh -s /usr/local/bin/bash
```

Then change for user 
```
chsh -s /usr/local/bin/bash nixcraft
```
