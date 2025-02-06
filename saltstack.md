# salt


[Istall link for linux](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/index.html)


## setup on debian 12


Install salt repos.

```
# Ensure keyrings dir exists
mkdir -p /etc/apt/keyrings
# Download public key
curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
# Create apt repo target configuration
curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
```

Populate `/etc/apt/preferences.d/salt-pin-1001` in order to restrict upgrades to Salt 3006.x LTS:

```
echo 'Package: salt-*
Pin: version 3006.*
Pin-Priority: 1001' | sudo tee /etc/apt/preferences.d/salt-pin-1001
```

Install:
```
sudo apt update
sudo apt-get install salt-master
sudo apt-get install salt-minion
sudo apt-get install salt-ssh
sudo apt-get install salt-syndic
sudo apt-get install salt-cloud
sudo apt-get install salt-api
```

Enable and start the Salt services
```
sudo systemctl enable salt-master && sudo systemctl start salt-master
sudo systemctl enable salt-minion && sudo systemctl start salt-minion
sudo systemctl enable salt-syndic && sudo systemctl start salt-syndic
sudo systemctl enable salt-api && sudo systemctl start salt-api
```

For more info for debian salt master setup see [debian](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html)

#### Setup salt master config

You can edit directly master.conf .
```
/etc/salt/minion.d/master.conf
```

But its better to add chages under.
```
/etc/salt/master.d/
```

Here is a exmaple config called `setup.conf` under `/etc/salt/master.d/`.

[Here is doc for diffrent file backends](https://docs.saltproject.io/en/3006/ref/file_server/backends.html).


[Pillar guide](https://docs.saltproject.io/salt/user-guide/en/latest/topics/pillar.html).


Create dir for salt states.
```
mkdir -p /srv/salt
```

You can git clone salt states.
```
git clone github.com/youruser/homelab_salt
```

Create dir for test.
```
mkdir -p /srv/salt/test
```


```
### mangaged by salt

#####      File Server settings      #####
##########################################

#You can have diffremt file_roots.

file_roots:
  base:
    - /srv/homelab_salt
  test:
    - /srv/salt/test


# you can choose diffrent file backends.
fileserver_backend:
  - roots
#  - git

#####         Pillar settings        #####
##########################################

pillar_roots:
  base:
    - /srv/pillar

#####         Git States and Pillar         #####
##########################################
#gitfs_provider: pygit2
#top_file_merging_strategy: same
#gitfs_pubkey: /root/.ssh/id_ed25519.pub
#gitfs_privkey: /root/.ssh/id_ed25519
#gitfs_remotes:
#  - ssh://git@github.com:habbfarm/habbfarm_salt.git
#gitfs_refspecs:
#  - '+refs/heads/*:refs/remotes/origin/*'

#ext_pillar:
#  - git:
#    - ssh://git@github.com/<user>/<repo>.git:
#      - env: '__env__'
#pillar_source_merging_strategy: none
#pillar_raise_on_missing: True
#git_pillar_provider: pygit2
#git_pillar_pubkey: /root/.ssh/id_ed25519.pub
#git_pillar_privkey: /root/.ssh/id_ed25519
```

### setup salt minion.

See install section.


Here you only need to install the package.
```
sudo apt-get install salt-minion 
```

Add salt minion config here.
```
/etc/salt/minion.d/master.conf
```

Example config add your salt master.
```
master: salt.local.net
```

Then enable salt-minion service.
```
systemctl enable --now salt-minion.service

systemctl status salt-minion.service
```

Login to salt master and check if you can see keys [link to doc](https://docs.saltproject.io/salt/install-guide/en/latest/topics/accept-keys.html#accept-keys).
```
salt-key
```
output
```
Unaccepted Keys:
db1
Accepted Keys:
web1
web2
Rejected:
badguy
```

To accep a key run.
```
salt-key -a db1
```

If there are multiple keys to accept and are trusted, you can accept all at once:
```
salt-key -A
```

Deleting key.
```
salt-key -d web1
```

Example of salt states it alway runs on the salt master [link to doc](https://docs.saltproject.io/salt/user-guide/en/latest/topics/states.html) .

Going to setup salt state for test like shown in the `/etc/salt/master.d/setup.conf`
```
cd /srv/salt/test
```
Create a top file called `top.sls`  this file defines role and to what server these role should run on.

[ffmuc-salt-public](https://github.com/freifunkMUC/ffmuc-salt-public)

```
# what environment server and role are under
# must be defined in salt master config.
test:
  # can be fulle fqdn
  'tv-t-salt-minion1.local.net':
    - webserver_setup
  # can be wildcard all server called 
  # something with tv-test will this role apply to.
  'tv-test*:
    - salt_minion
```

Create folder for salt state.

```
mkdir -p /srv/salt/test/salt_minion
```

Example salt state.

```
vim /srv/salt/test/salt_minion/init.sls
```

```
# Make sure salt is installed and running
#
salt_master:
  pkg.installed:
    - name: salt-master
  service.running:
    - name: salt-master
    - require:
      - pkg: salt-master
    - watch:
      - file: /etc/apt/sources.list.d/salt.sources
      - file: /etc/salt/minion.d/master.conf
      - file: /etc/salt/master.d/setup.conf

/etc/salt/gpgkeys:
  file.directory:
    - user: root
    - group: root
    - mode: 700
    - makedirs: True

/srv/salt/test:
  file.directory:
    - user: root
    - group: root
    - mode: 644
    - makedirs: True

/srv/pillar:
  file.directory:
    - user: root
    - group: root
    - mode: 644
    - makedirs: True

/etc/salt/minion.d/master.conf:
  file.managed:
    - source: salt://salt_master/templates/master.conf
    - user: root
    - group: root
    - mode: 644

/etc/salt/master.d/setup.conf:
  file.managed:
    - source: salt://salt_master/templates/setup.conf
    - user: root
    - group: root
    - mode: 644

/etc/salt/gpgkeys/gen-key-script:
  file.managed:
    - source: salt://salt_master/templates/gen-key-script
    - user: root
    - group: root
    - mode: 644

{% if not salt['file.file_exists' ]('/etc/salt/gpgkeys/pubring.kbx') %}
salt_create_gpg_key:
  cmd.run:
    - name: "gpg --homedir /etc/salt/gpgkeys  --batch --gen-key /etc/salt/gpgkeys/gen-key-script"
{% endif %}

{% if not salt['file.file_exists' ]('/root/.ssh/id_ed25519') %}
salt_create_ssh_key:
  cmd.run:
    - name: "ssh-keygen -t ed25519 -f /root/.ssh/id_ed25519 -N """
{% endif %}
```

Using salt with gpg to encrypt variables.

[salt pillar data encryption](https://fabianlee.org/2016/10/18/saltstack-keeping-salt-pillar-data-encrypted-using-gpg/)





