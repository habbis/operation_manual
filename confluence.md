# Confluence install guide 

Create a user to run confluence
```
useradd --home-dir /opt/confluence --shell /bin/nologin youruser
```

## Prepare shell
For convenience
```
CONFUSER=youruser
```


## Partitioning
Confluence will be installed under /opt , the minimum recommended size is 5G to have space for multiple versions during upgrades.
```
lvextend -rL5G /dev/vg0/Opt
```
