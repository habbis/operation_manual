# etherpad install guide

First cd to /opt then we make dirs to run the application
```
mkdir etherpad
mkdir node
```
cd etherpad and either git clone or download the zip file
```
git clone --branch master git://github.com/ether/etherpad-lite.git

ln -s etherpad-lite current 
````
Or we use the zip file
```
https://github.com/ether/etherpad-lite/archive/1.8.4.zip

unzip 1.8.4.zip

ln - etherpad-lite-1.8.4 current
```

create a ordinary user to run etherpad
```
useradd -m /opt/etherpad/current/ --shell /bin/sh youruser
```

To give the etherpad user access to dir
```
chown -R youruser:youruser /opt/etherpad/current

useradd -m /opt/etherpad/current/ --shell /bin/nologin test04
```
To test run etherpad 
```
sudo su - youruser -s /bin/sh -c /opt/etherpad-lite/bin/run.sh
```
cd node then download nodejs lts
```
wget https://nodejs.org/dist/v12.16.3/node-v12.16.3-linux-x64.tar.xz

# if missing xf-utils 
 apt install -y  xz-utils
 
tar xJvf node-v12.16.3.tar.xz

ln -s node-v12.16.3 current  
```
Then we symlink npm nad node in working dir
```
ln -s /opt/node-v12.16.3-linux-x64/bin/npm /usr/bin/npm 
ln -s /opt/node-v12.16.3-linux-x64/bin/node /usr/bin/node
```

 To set etherpad i production mode
```
export NODE_ENV=production
```

To start etherpad
```
/opt/etherpad-lite/bin/run.sh
```

It will create a settings.json at 

```
 ls -al /opt/etherpad-lite/ |grep settings.json |grep -v settings.json.
-rw-r--r--  1 test03 test03 16790 May 21 20:56 settings.json

```

To create a older type init shell script 
place it in /etc/init.d/ and call it ehterpad.

And to make it to run
```
update-rc.d etherpad-lite defaults
```

```
#!/bin/sh

### BEGIN INIT INFO
# Provides:          etherpad-lite
# Required-Start:    $local_fs $remote_fs $network $syslog
# Required-Stop:     $local_fs $remote_fs $network $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: starts etherpad lite
# Description:       starts etherpad lite using start-stop-daemon
### END INIT INFO

PATH="/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/opt/node/bin"
LOGFILE="/var/log/etherpad-lite/etherpad-lite.log"
EPLITE_DIR="/usr/share/etherpad-lite"
EPLITE_BIN="bin/safeRun.sh"
USER="etherpad-lite

"
GROUP="etherpad-lite"
DESC="Etherpad Lite"
NAME="etherpad-lite"

set -e

. /lib/lsb/init-functions

start() {
  echo "Starting $DESC... "
  
	start-stop-daemon --start --chuid "$USER:$GROUP" --background --make-pidfile --pidfile /var/run/$NAME.pid --exec $EPLITE_DIR/$EPLITE_BIN -- $LOGFILE || true
  echo "done"
}

#We need this function to ensure the whole process tree will be killed
killtree() {
    local _pid=$1
    local _sig=${2-TERM}
    for _child in $(ps -o pid --no-headers --ppid ${_pid}); do
        killtree ${_child} ${_sig}
    done
    kill -${_sig} ${_pid}
}

stop() {
  echo "Stopping $DESC... "
  if test -f /var/run/$NAME.pid; then
    while test -d /proc/$(cat /var/run/$NAME.pid); do
      killtree $(cat /var/run/$NAME.pid) 15
      sleep 0.5
    done
    rm /var/run/$NAME.pid
  fi
  echo "done"
}

status() {
  status_of_proc -p /var/run/$NAME.pid "" "etherpad-lite" && exit 0 || exit $?
}

case "$1" in
  start)
	  start
	  ;;
  stop)
    stop
	  ;;
  restart)
	  stop
	  start
	  ;;
  status)
	  status
	  ;;
  *)
	  echo "Usage: $NAME {start|stop|restart|status}" >&2
	  exit 1
	  ;;
esac

exit 0
```

Create a systemd service 

From developers:

With systemd it is better not to use the run.sh but to let systemd run node directly.

Important! A manual run of bin/installDeps.sh is needed before the first start as service.

etherpad.service

```
[Unit]
Description=Etherpad-lite, the collaborative editor.
After=syslog.target network.target

[Service]
Type=simple
User=etherpad
Group=etherpad
WorkingDirectory=/opt/etherpad
Environment=NODE_ENV=production
ExecStart=/usr/bin/nodejs /opt/etherpad/node_modules/ep_etherpad-lite/node/server.js
Restart=always # use mysql plus a complete settings.json to avoid Service hold-off time over, scheduling restart.

[Install]
WantedBy=multi-user.target
```

links:

[etherpad service](https://github.com/ether/etherpad-lite/wiki/How-to-deploy-Etherpad-Lite-as-a-service)
