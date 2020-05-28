
# gogs install guide

First install git 

debian/ubuntu
```
apt install -y git
```
RHEL/centos 
```
yum install -y git
```

Then cd to /opt then we make dirs to run the application
```
mkdir gogs
```
cd etherpad and either git clone or download the zip file
```
wget https://dl.gogs.io/0.11.91/gogs_0.11.91_linux_amd64.tar.gz

tar -zxf gogs_0.11.91_linux_amd64.tar.gz

mv gogs gogs_yourversion

ln -s  current 
````
create a ordinary user to run gogs
```
useradd -m /opt/etherpad/current/ --shell /bin/sh youruser
```

To give the gogs user access to dir
```
chown -R youruser:youruser /opt/etherpad/current
```

Init script setup for gogs.

Under script folder you can find diffrent types fo  init script for linux distros or unix os like freebsd.
```
root@repo01:/opt/gogs/current# ls -al /opt/gogs/current/scripts/                                              [0/1391]
total 47
drwxr-xr-x 7 user user  13 May 27 23:51 .
drwxr-xr-x 5 user user   9 Aug 12  2019 ..
-rw-r--r-- 1 user user  92 Jun  5  2018 README
-rwxr-xr-x 1 user user  73 Jun  5  2018 autoboot.sh
-rwxr-xr-x 1 user user 747 Jun  5  2018 build.sh
-rwxr-xr-x 1 user user 679 Jun  5  2018 build_freebsd.sh
-rwxr-xr-x 1 user user 692 Jun  5  2018 build_linux64.sh
drwxr-xr-x 8 user user   8 Aug 12  2019 init
drwxr-xr-x 2 user user   3 Aug 12  2019 launchd
-rw-r--r-- 1 user user 234 Jun  5  2018 mysql.sql
drwxr-xr-x 2 user user   3 Aug 12  2019 supervisor
drwxr-xr-x 2 user user   3 Aug 12  2019 systemd
drwxr-xr-x 2 user user   3 Aug 12  2019 windows
```
To setup like a old sysinit way you can copy the script..
```
cp /opt/gogs/current/scripts/init/debian/gogs /etc/init.d/gogs
```
Here is wat the script sysinit script lookslike
```
vim  /etc/init.d/gogs

#! /bin/sh
### BEGIN INIT INFO
# Provides:          gogs
# Required-Start:    $syslog $network
# Required-Stop:     $syslog
# Should-Start:      mysql postgresql
# Should-Stop:       mysql postgresql
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: A self-hosted Git service written in Go.
# Description:       A self-hosted Git service written in Go.
### END INIT INFO

# Author: Danny Boisvert

# Do NOT "set -e"

# PATH should only include /usr/* if it runs after the mountnfs.sh script
PATH=/sbin:/usr/sbin:/bin:/usr/bin
DESC="Gogs"
NAME=gogs
SERVICEVERBOSE=yes
PIDFILE=/var/run/$NAME.pid
SCRIPTNAME=/etc/init.d/$NAME
WORKINGDIR=/opt/gogs/current/
DAEMON=$WORKINGDIR/$NAME
DAEMON_ARGS="web"
USER=youruser

# Read configuration variable file if it is present
[ -r /etc/default/$NAME ] && . /etc/default/$NAME

# Exit if the package is not installed
[ -x "$DAEMON" ] || exit 0

# Load the VERBOSE setting and other rcS variables
. /lib/init/vars.sh

# Define LSB log_* functions.
# Depend on lsb-base (>= 3.2-14) to ensure that this file is present
# and status_of_proc is working.
. /lib/lsb/init-functions

#
# Function that starts the daemon/service
#
do_start()
{
        # Return
        #   0 if daemon has been started
        #   1 if daemon was already running
        #   2 if daemon could not be started
        sh -c "USER=$USER start-stop-daemon --start --quiet --pidfile $PIDFILE --make-pidfile \\
                        --test --chdir $WORKINGDIR --chuid $USER \\
                        --exec $DAEMON -- $DAEMON_ARGS > /dev/null \\
                        || return 1"
        sh -c "USER=$USER start-stop-daemon --start --quiet --pidfile $PIDFILE --make-pidfile \\
                        --background --chdir $WORKINGDIR --chuid $USER \\
                        --exec $DAEMON -- $DAEMON_ARGS \\
                        || return 2"
}

#
# Function that stops the daemon/service
#
do_stop()
{
        # Return
        #   0 if daemon has been stopped
        #   1 if daemon was already stopped
        #   2 if daemon could not be stopped
        #   other if a failure occurred
        start-stop-daemon --stop --quiet --retry=TERM/1/KILL/5 --pidfile $PIDFILE --name $NAME
        RETVAL="$?"
        [ "$RETVAL" = 2 ] && return 2
        start-stop-daemon --stop --quiet --oknodo --retry=0/1/KILL/5 --exec $DAEMON
        [ "$?" = 2 ] && return 2
        # Many daemons don't delete their pidfiles when they exit.
        rm -f $PIDFILE
        return "$RETVAL"
}


case "$1" in
  start)
        [ "$SERVICEVERBOSE" != no ] && log_daemon_msg "Starting $DESC" "$NAME"
        do_start
        case "$?" in
                0|1) [ "$SERVICEVERBOSE" != no ] && log_end_msg 0 ;;
                2) [ "$SERVICEVERBOSE" != no ] && log_end_msg 1 ;;
        esac
        ;;
  stop)
        [ "$SERVICEVERBOSE" != no ] && log_daemon_msg "Stopping $DESC" "$NAME"
        do_stop
        case "$?" in
                0|1) [ "$SERVICEVERBOSE" != no ] && log_end_msg 0 ;;
                2) [ "$SERVICEVERBOSE" != no ] && log_end_msg 1 ;;
        esac
        ;;
  status)
        status_of_proc "$DAEMON" "$NAME" && exit 0 || exit $?
        ;;
  restart|force-reload)
        log_daemon_msg "Restarting $DESC" "$NAME"
        do_stop
        case "$?" in
          0|1)
                do_start
                case "$?" in
                        0) log_end_msg 0 ;;
                        1) log_end_msg 1 ;; # Old process is still running
                        *) log_end_msg 1 ;; # Failed to start
                esac
                ;;
          *)
                # Failed to stop
                log_end_msg 1
                ;;
                esac
        ;;
  *)
                echo "Usage: $SCRIPTNAME {start|stop|status|restart|force-reload}" >&2
                exit 3
                ;;
esac                  
```
Make init script executable and add it to start at boot.
```
chmod +x /etc/init.d/confluence
chkconfig --add gogs
```


Or you can user the systemd service 
```
root@repo01:/opt/gogs/current# cat /opt/gogs/current/scripts/systemd/gogs.service
[Unit]
Description=Gogs
After=syslog.target
After=network.target
After=mariadb.service mysqld.service postgresql.service memcached.service redis.service

[Service]
# Modify these two values and uncomment them if you have
# repos with lots of files and get an HTTP error 500 because
# of that
###
#LimitMEMLOCK=infinity
#LimitNOFILE=65535
Type=simple
User=youruser 
Group=youruser
WorkingDirectory=/opt/current/
ExecStart=/home/git/gogs/gogs web
Restart=always
Environment=USER=git HOME=/home/git

# Some distributions may not support these hardening directives. If you cannot start the service due                 
# to an unknown option, comment out the ones not supported by your version of systemd.
ProtectSystem=full
PrivateDevices=yes
PrivateTmp=yes
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target

```

Links:

[gogs download](https://gogs.io/docs/installation/install_from_binary)


