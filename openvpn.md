# openvpn 




## openvpn client

To connect a openvpn client at boot on linux you can do it like this.

Create a folder i am creating `/opt/scripts/openvpn` here is the scritps stored and config 

Upload your config file and rename it to with .conf at the end.
```
yourvpn.conf
```

Then create .txt file with the credentilas.
```
yourvpn_credentials.txt
```
It looks like this on the inside.
```
youruser
youruserpass
```

Then create a openvpn client script called openvpn_client
```
#/bin/bash
/usr/sbin/openvpn --config /opt/scripts/yourvpn.conf </dev/null &>/dev/null &
```

Then we create a init script under `/etc/init.d/` called yourvpn .
```
#!/bin/sh -e
# openvpn init script to connect to a vpn when booting
# Define some variables
# Name of app
APP=yourvpn
# Name of the user to run as
USER=root
# Location of install directory
PROGRAM=/opt/scripts
PROGRAM02=/usr/bin
case "$1" in
# Start command
start)
echo "Starting $APP"
/bin/su -m $USER -c "$PROGRAM/openvpn_client.sh &> /dev/null"
;;
# Stop command
stop)
echo "Stopping $APP"
/bin/su -m $USER -c "$PROGRAM02/pkill openvpn &> /dev/null"
echo "$APP stopped successfully"
;;
# Restart command
restart)
$0 stop
sleep 5
$0 start
;;
*)
echo "Usage: /etc/init.d/$APP {start|restart|stop}"
exit 1
;;
esac
exit 0

```

remember to make the scripts executable `chmod +x`
