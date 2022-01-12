# python



## installing python from source 

Installing extra packages for installing python3.10 form source.

Rhel based.
```
yum install gcc openssl-devel libffi-devel bzip2-devel wget
```

Debian based.
```
apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
```

Going to path here i will download python3.10 .
```
cd /usr/local/
```

Downloading python3.10 .
```
wget https://www.python.org/ftp/python/3.10.1/Python-3.10.1.tgz
```
Untar the file.
```
tar xzvf Python-3.10.1.tgz
```
Cd into the python3.10 dir.
```
cd Python-3.10.1
```
Compile and install python3.10 .
```
./configure --enable-optimizations
make altinstall
```

After its done it should be like this.
```
[root@eqkh-ora-01 Python-3.10.1]# python3.10 --version
Python 3.10.1
```

Added alias to /etc/profile .
```
alias python3='python3.10'
```
You can use python3.10 like this.

python3  or python3.10

To install pip packages.
```
python3 -m pip install yourpackage  or python3.10 -m pip install yourpackage
```



