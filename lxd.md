# lxd

Here is a guide to setup lxd. warning this guide can be outdated a new guide will maybe come late.
[lxd setup](https://habbis.github.io/post/lxd/)

ways to user cloud-init to setup lxc container 
```

So there are multiple ways you can do so, either directly for a container:

lxc init ubuntu: CONTAINER
lxc config set CONTAINER user.user-data - < cloud-init-config.yml
lxc start CONTAINER

Or even shorter:

lxc launch ubuntu: CONTAINER --config=user.user-data="$(cat cloud-init-config.yml)"

Or through a profile:

lxc profile create dev
lxc profile set dev user.user-data - < cloud-init-config.yml
lxc launch ubuntu: CONTAINER -p default -p dev


```
run docker inside a lxc container managed by lxd use nesting and this is a similar thing like vm nesting but this is not hardware based like with a hardware virtualization.

To launch a container with nesting 

```
lxc launch ubuntu nestc1 -c security.nesting=true 
```
If you want a priviliged container(its not recommended to user privliged)
```
lxc launch ubuntu nestc1 -c security.nesting=true -c security.privileged=true
```
You may need to add more UIDs and GIDs to lxd so see
[Custom user mappings in LXD containers](https://stgraber.org/2017/06/15/custom-user-mappings-in-lxd-containers/)

Here is what is look like.

```
stgraber@castiana:~$ cat /etc/subuid
lxd:100000:65536
root:100000:65536

stgraber@castiana:~$ cat /etc/subgid
lxd:100000:65536
root:100000:65536

The maps for “lxd” and “root” should always be kept in sync. LXD itself is restricted by the “root” allocation. The “lxd” entry is used to track what needs to be removed if LXD is uninstalled.

Now if you want to increase the size of the map available to LXD. Simply edit both of the files and bump the last value from 65536 to whatever size you need. I tend to bump it to a billion just so I don’t ever have to think about it again:

stgraber@castiana:~$ cat /etc/subuid
lxd:100000:1000000000
root:100000:1000000000

stgraber@castiana:~$ cat /etc/subgid
lxd:100000:1000000000
root:100000:100000000
```

How a lxc profile can look like.

To make a new profle
```
lxc profile copy default youname
```
To edit 
```
lxc profle edit yourprofile
```

To setup bridge with vlan on netplan this works on ubuntu 18.04 and 20.04
[ubuntu bridge setup](https://github.com/habbis/ubuntu_bridge_setup/blob/master/00-bridge_vlan.yaml)

How a config can look like with bridge setup.

```
### This is a yaml representation of the profile.
### Any line starting with a '# will be ignored.
###
### A profile consists of a set of configuration items followed by a set of
### devices.
###
### An example would look like:
### name: onenic
### config:
###   raw.lxc: lxc.aa_profile=unconfined
### devices:
###   eth0:
###     nictype: bridged
###     parent: lxdbr0
###     type: nic
###
### Note that the name is shown but cannot be changed

config:{}
description: Default LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: br0

```
With cloud-init setup so you can launch a container with the config you want
```
### This is a yaml representation of the profile.
### Any line starting with a '# will be ignored.
###
### A profile consists of a set of configuration items followed by a set of
### devices.
###
### An example would look like:
### name: onenic
### config:
###   raw.lxc: lxc.aa_profile=unconfined
### devices:
###   eth0:
###     nictype: bridged
###     parent: lxdbr0
###     type: nic
###
### Note that the name is shown but cannot be changed

config:
  user.user-data: |
    #cloud-config
    users:
      - name: root
        ssh-authorized-keys:
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCw0246LpRtBBJV4t9gsPRaGy5ZMfJAZ63fHN7EdBzZvWO6jrS5BdRGujt50sykXo54mpkbrKCngWqz+q5glqbiyXBVhygB0K1IGILFuxKHiShBsynM/2q613VQeMDNqs6PCzToS8z2Dyz6Axo/45vIfRfd+ruhVaeC2J/bzoka3Uq1as5MwGNB+FKlirhqUDJHUFEE2cqFj8O2hnOj4KwtwrTfpc+689Uxg4MnlKdT6bgLKcp49vg9rmQoTkdWknxZ8UcSG9QR/Y4ObsFJicYJx8XIDJUBXvIoqJd0udW+TfOkIiKStUQy9AAbWRAjz3qbZXeDCASOJA5H8a8Nf98/
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDznoT7SGEe/Yld35/FmQF0UWDkn/k4zZLggisEbnjr8xgWW0pNMvwt7gH9GvCQtTb5jDj2VYlTah52/00E73mD28OfY/8L1Lu9bs3mtkh0jpeY2C7+qPWgVG0kVQE62InEsK5DfhjI6BUDajHVucvdNr0BERax8J9BCQM45uG62O2JqBkFGsvbJsleeAbuPavYIi9dAqs/A9OBjlib0r5ysXjIkC0YnG99T9bRsyuCdefvSD8pLr7aM0UPRpZZqxYmexr4WLocChqCwR9DO+GCnLwZep4VL2n/uK9BDe0awXhZ/MJDMhpFAZsSJZG5TQk7Ry5n7sfRUozsjW0D66If
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDKowVHY6Y5Ar+AYAUeCJiMHH+UQNUZjN8De5dc/GKZOROIKYRxHAaPIgtvoHNNSU7w30p3gCvJaeFrqXr/HXFWJOp+2lRE6QDWIM8WqAJNwL87es7OAEyx8H+WLxoLpp/sYLAwV5xe49RGDrMJUfW6Rw2mLDiojlddB+t4L+fugPAb5K2UlxOAQ8ZD56hzcDAWwlADJ2+cavWqRrlsANgKJWrLTE0N9N/lcr9l3ar4W/ZP1Dq16d6s3pO2JVwcDdtz0jcOBhPnCVpIJIt0oykUU2IwiF9LErLZdB4Qm+2H1CM3X/RP9cd8aEZ6gmMmkTu4mT5ge4yWm7w50vj7INgqkyCBwEPRk9d2Cmh9maDr5sU8FyLnD8MnvNu80LwqfTaIA9NAI6VpVAuvTjrLHVaV4+KG50NcnG9Mw6Z8O6T8Cx7k1bDAXelarpA6fGNFkXkECC2mFdDvPVqRZmVvij/b5qEYToSOfsJytOLMFPZCnUQ/CRWsZMRyMSrokNQ10AmScN9rhAErbx4/c+utfEmtEpNBm2t9z2GiVvkLyGycN09gnVmlNkgv5xojAO4HPKdcfAml00lzu8ydtYhSyo7q91XImRbZSlyB360K7JZ0qTCIzPObqbQGwCMQOZ2Tt9+plqLuCQfdQiF0w9xOiufDCs+zdDO3FMuxg3e6c9mmew==
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCtrbOooF4bEBdymEB9N5jh2mDCLAmWO5TCh0gorNOWyC5htBAJ0gqHGdh6OHqg2wT5sLDSMQ/FrpaOIdW5ULitN3aL+LTNDEhrU9mHTsPTlU2Bh7Fl6pnanQp7peUiEzbi7W7jRaC/IwDQwZh1tvSX2sUOVk+0nz7MtJXiyp3gfexltvySrSCHPRe4zZRrMu9PZ/LiaSJ0NTyCP/xEDYhXgV3g0R6YqRxiaXBzi/8Y4EgAYx/upekvB8OnVxYKXt6B+2Gqx4K1/GribiqL6ymevDUja0DIXumD6POqohbtjB3b+yka2pFoydnB0hkquEo7J9aB41DIszyePjci3Ur3ncbWJkS1Z5nkaAlVCziO8jLbBaQnVxVjE7QIXvqGePQRsMV3u6E5jGeJDq1Ub8Ynm91aX6iwrj3POhGIlxcJMMQkxc+2o+I23xNY22aoUXt5j9B543U2Ygsq3I3JPtNLFJbnIFQQv0zpviqaCj7daDc58QK6AHoskuWMEnrsI2zXbJjCpxSiJp5eAtXHfkZT5n6JdEZrgWnke6+Bvtu43kLSLxhLG5fXNOgZhKpZLd5NOv2/tL1M8ir/OtFsWdAvtnE5zykK7EZ157/Pdv82afAaxcOwkjwHgNNTb4/TOI68GjPZubm3ue9Frzcdt3E6wb+XAduvW767dauDWuUqqQ==
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDXiyYEUHQOC2T9OQol52MCTaZFE1UFvBcsrl4NjgURvVU5BjP6IQF+1E7o9DqDGRZOj1ddHR4SfVepGvXjomUyGYi7N3ii/lZfHvua0S5BUOua+kfLm2OWKkBqYlZOk1K99OGIIavS6xnMULIkwv01TmcuJeyBPjrioGZhqkUpHdWv2NsFQjVyuu9rr8rvwcH1DEqLlzwrfrMYEfFASMQ6DXv/IiZJGBol613RfgYxRh1qfURO8aPe3twus8MAhTiZ60KEVOUkH0Br8L9CxHYqGu+qnmiLYnD0CYyj2JTF/UJ8qcipl1SOdSvCu5A07SJuBMzDJu7qxpORpoVFnuWQInpJUJvLsAOZ4TNiAL842+yF2eNl+ORC3HY6AwTktCEldFILCAVTFigAMPzHM+KoULPABzH9mVhhOVYDLWRPYm0Zcbyj9PwK6WBz1+QalxFnMRTWCZTjRkyurejcvvJ0MCCacFbwP7t0/LOnML5A1ee5K4A1rviSOIJcjXIn9E4JHsGgl3ZkPQ56YGEYTwCclnj8IxbupKJVMasE+bM+UviZhjT9cfvBgwkTZPwBGc0tw7i6sU5JB1gYkBeR79NJERFLRqRdyog2T/0TpahBPtZAGEz9g4UcosMrxqfrNE59GrYuQL5qn1e9tGWFQdwkwLeBrEDXP0VoP/Y5R5f4pQ==
        sudo: ['ALL=(ALL) NOPASSWD:ALL']
        groups: sudo
        shell: /bin/bash
      - name: user
        ssh-authorized-keys:
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCw0246LpRtBBJV4t9gsPRaGy5ZMfJAZ63fHN7EdBzZvWO6jrS5BdRGujt50sykXo54mpkbrKCngWqz+q5glqbiyXBVhygB0K1IGILFuxKHiShBsynM/2q613VQeMDNqs6PCzToS8z2Dyz6Axo/45vIfRfd+ruhVaeC2J/bzoka3Uq1as5MwGNB+FKlirhqUDJHUFEE2cqFj8O2hnOj4KwtwrTfpc+689Uxg4MnlKdT6bgLKcp49vg9rmQoTkdWknxZ8UcSG9QR/Y4ObsFJicYJx8XIDJUBXvIoqJd0udW+TfOkIiKStUQy9AAbWRAjz3qbZXeDCASOJA5H8a8Nf98/
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDznoT7SGEe/Yld35/FmQF0UWDkn/k4zZLggisEbnjr8xgWW0pNMvwt7gH9GvCQtTb5jDj2VYlTah52/00E73mD28OfY/8L1Lu9bs3mtkh0jpeY2C7+qPWgVG0kVQE62InEsK5DfhjI6BUDajHVucvdNr0BERax8J9BCQM45uG62O2JqBkFGsvbJsleeAbuPavYIi9dAqs/A9OBjlib0r5ysXjIkC0YnG99T9bRsyuCdefvSD8pLr7aM0UPRpZZqxYmexr4WLocChqCwR9DO+GCnLwZep4VL2n/uK9BDe0awXhZ/MJDMhpFAZsSJZG5TQk7Ry5n7sfRUozsjW0D66If
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDKowVHY6Y5Ar+AYAUeCJiMHH+UQNUZjN8De5dc/GKZOROIKYRxHAaPIgtvoHNNSU7w30p3gCvJaeFrqXr/HXFWJOp+2lRE6QDWIM8WqAJNwL87es7OAEyx8H+WLxoLpp/sYLAwV5xe49RGDrMJUfW6Rw2mLDiojlddB+t4L+fugPAb5K2UlxOAQ8ZD56hzcDAWwlADJ2+cavWqRrlsANgKJWrLTE0N9N/lcr9l3ar4W/ZP1Dq16d6s3pO2JVwcDdtz0jcOBhPnCVpIJIt0oykUU2IwiF9LErLZdB4Qm+2H1CM3X/RP9cd8aEZ6gmMmkTu4mT5ge4yWm7w50vj7INgqkyCBwEPRk9d2Cmh9maDr5sU8FyLnD8MnvNu80LwqfTaIA9NAI6VpVAuvTjrLHVaV4+KG50NcnG9Mw6Z8O6T8Cx7k1bDAXelarpA6fGNFkXkECC2mFdDvPVqRZmVvij/b5qEYToSOfsJytOLMFPZCnUQ/CRWsZMRyMSrokNQ10AmScN9rhAErbx4/c+utfEmtEpNBm2t9z2GiVvkLyGycN09gnVmlNkgv5xojAO4HPKdcfAml00lzu8ydtYhSyo7q91XImRbZSlyB360K7JZ0qTCIzPObqbQGwCMQOZ2Tt9+plqLuCQfdQiF0w9xOiufDCs+zdDO3FMuxg3e6c9mmew==
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCtrbOooF4bEBdymEB9N5jh2mDCLAmWO5TCh0gorNOWyC5htBAJ0gqHGdh6OHqg2wT5sLDSMQ/FrpaOIdW5ULitN3aL+LTNDEhrU9mHTsPTlU2Bh7Fl6pnanQp7peUiEzbi7W7jRaC/IwDQwZh1tvSX2sUOVk+0nz7MtJXiyp3gfexltvySrSCHPRe4zZRrMu9PZ/LiaSJ0NTyCP/xEDYhXgV3g0R6YqRxiaXBzi/8Y4EgAYx/upekvB8OnVxYKXt6B+2Gqx4K1/GribiqL6ymevDUja0DIXumD6POqohbtjB3b+yka2pFoydnB0hkquEo7J9aB41DIszyePjci3Ur3ncbWJkS1Z5nkaAlVCziO8jLbBaQnVxVjE7QIXvqGePQRsMV3u6E5jGeJDq1Ub8Ynm91aX6iwrj3POhGIlxcJMMQkxc+2o+I23xNY22aoUXt5j9B543U2Ygsq3I3JPtNLFJbnIFQQv0zpviqaCj7daDc58QK6AHoskuWMEnrsI2zXbJjCpxSiJp5eAtXHfkZT5n6JdEZrgWnke6+Bvtu43kLSLxhLG5fXNOgZhKpZLd5NOv2/tL1M8ir/OtFsWdAvtnE5zykK7EZ157/Pdv82afAaxcOwkjwHgNNTb4/TOI68GjPZubm3ue9Frzcdt3E6wb+XAduvW767dauDWuUqqQ==
          - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDXiyYEUHQOC2T9OQol52MCTaZFE1UFvBcsrl4NjgURvVU5BjP6IQF+1E7o9DqDGRZOj1ddHR4SfVepGvXjomUyGYi7N3ii/lZfHvua0S5BUOua+kfLm2OWKkBqYlZOk1K99OGIIavS6xnMULIkwv01TmcuJeyBPjrioGZhqkUpHdWv2NsFQjVyuu9rr8rvwcH1DEqLlzwrfrMYEfFASMQ6DXv/IiZJGBol613RfgYxRh1qfURO8aPe3twus8MAhTiZ60KEVOUkH0Br8L9CxHYqGu+qnmiLYnD0CYyj2JTF/UJ8qcipl1SOdSvCu5A07SJuBMzDJu7qxpORpoVFnuWQInpJUJvLsAOZ4TNiAL842+yF2eNl+ORC3HY6AwTktCEldFILCAVTFigAMPzHM+KoULPABzH9mVhhOVYDLWRPYm0Zcbyj9PwK6WBz1+QalxFnMRTWCZTjRkyurejcvvJ0MCCacFbwP7t0/LOnML5A1ee5K4A1rviSOIJcjXIn9E4JHsGgl3ZkPQ56YGEYTwCclnj8IxbupKJVMasE+bM+UviZhjT9cfvBgwkTZPwBGc0tw7i6sU5JB1gYkBeR79NJERFLRqRdyog2T/0TpahBPtZAGEz9g4UcosMrxqfrNE59GrYuQL5qn1e9tGWFQdwkwLeBrEDXP0VoP/Y5R5f4pQ==
        sudo: ['ALL=(ALL) NOPASSWD:ALL']
        groups: sudo
        shell: /bin/bash
description: Default LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: br0

```









links:

[configure lxd containers with cloud config](https://askubuntu.com/questions/617865/is-there-a-way-to-configure-lxd-containers-with-cloud-config-at-provision-time)
[Nested containers in LXD](https://ubuntu.com/blog/nested-containers-in-lxd)
