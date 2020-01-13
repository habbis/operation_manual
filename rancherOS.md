# Setup RancherOS
Make a vm on your favorit hypervisor 
Loging to RancherOS and make a cloud-config.yml

vi cloud-config.yml

It can look like this 
```
#cloud-config

hostname: rancher

rancher:
  network:
    interfaces:
      eth*:
       dhcp: false
      eth0:
       address: 192.168.195.138/24
       gateway: 192.168.195.2
    dns:
     nameservers:
      - 8.8.8.8

ssh_authorized_keys:
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCw0246LpRtBBJV4t9gsPRaGy5ZMfJAZ63fHN7EdBzZvWO6jrS5BdRGujt50sykXo54mpkbrKCngWqz+q5glqbiyXBVhygB0K1IGILFuxKHiShBsynM/2q613VQeMDNqs6PCzToS8z2Dyz6Axo/45vIfRfd+ruhVaeC2J/bzoka3Uq1as5MwGNB+FKlirhqUDJHUFEE2cqFj8O2hnOj4KwtwrTfpc+689Uxg4MnlKdT6bgLKcp49vg9rmQoTkdWknxZ8UcSG9QR/Y4ObsFJicYJx8XIDJUBXvIoqJd0udW+TfOkIiKStUQy9AAbWRAjz3qbZXeDCASOJA5H8a8Nf98/
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDznoT7SGEe/Yld35/FmQF0UWDkn/k4zZLggisEbnjr8xgWW0pNMvwt7gH9GvCQtTb5jDj2VYlTah52/00E73mD28OfY/8L1Lu9bs3mtkh0jpeY2C7+qPWgVG0kVQE62InEsK5DfhjI6BUDajHVucvdNr0BERax8J9BCQM45uG62O2JqBkFGsvbJsleeAbuPavYIi9dAqs/A9OBjlib0r5ysXjIkC0YnG99T9bRsyuCdefvSD8pLr7aM0UPRpZZqxYmexr4WLocChqCwR9DO+GCnLwZep4VL2n/uK9BDe0awXhZ/MJDMhpFAZsSJZG5TQk7Ry5n7sfRUozsjW0D66If
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDKowVHY6Y5Ar+AYAUeCJiMHH+UQNUZjN8De5dc/GKZOROIKYRxHAaPIgtvoHNNSU7w30p3gCvJaeFrqXr/HXFWJOp+2lRE6QDWIM8WqAJNwL87es7OAEyx8H+WLxoLpp/sYLAwV5xe49RGDrMJUfW6Rw2mLDiojlddB+t4L+fugPAb5K2UlxOAQ8ZD56hzcDAWwlADJ2+cavWqRrlsANgKJWrLTE0N9N/lcr9l3ar4W/ZP1Dq16d6s3pO2JVwcDdtz0jcOBhPnCVpIJIt0oykUU2IwiF9LErLZdB4Qm+2H1CM3X/RP9cd8aEZ6gmMmkTu4mT5ge4yWm7w50vj7INgqkyCBwEPRk9d2Cmh9maDr5sU8FyLnD8MnvNu80LwqfTaIA9NAI6VpVAuvTjrLHVaV4+KG50NcnG9Mw6Z8O6T8Cx7k1bDAXelarpA6fGNFkXkECC2mFdDvPVqRZmVvij/b5qEYToSOfsJytOLMFPZCnUQ/CRWsZMRyMSrokNQ10AmScN9rhAErbx4/c+utfEmtEpNBm2t9z2GiVvkLyGycN09gnVmlNkgv5xojAO4HPKdcfAml00lzu8ydtYhSyo7q91XImRbZSlyB360K7JZ0qTCIzPObqbQGwCMQOZ2Tt9+plqLuCQfdQiF0w9xOiufDCs+zdDO3FMuxg3e6c9mmew==
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCtrbOooF4bEBdymEB9N5jh2mDCLAmWO5TCh0gorNOWyC5htBAJ0gqHGdh6OHqg2wT5sLDSMQ/FrpaOIdW5ULitN3aL+LTNDEhrU9mHTsPTlU2Bh7Fl6pnanQp7peUiEzbi7W7jRaC/IwDQwZh1tvSX2sUOVk+0nz7MtJXiyp3gfexltvySrSCHPRe4zZRrMu9PZ/LiaSJ0NTyCP/xEDYhXgV3g0R6YqRxiaXBzi/8Y4EgAYx/upekvB8OnVxYKXt6B+2Gqx4K1/GribiqL6ymevDUja0DIXumD6POqohbtjB3b+yka2pFoydnB0hkquEo7J9aB41DIszyePjci3Ur3ncbWJkS1Z5nkaAlVCziO8jLbBaQnVxVjE7QIXvqGePQRsMV3u6E5jGeJDq1Ub8Ynm91aX6iwrj3POhGIlxcJMMQkxc+2o+I23xNY22aoUXt5j9B543U2Ygsq3I3JPtNLFJbnIFQQv0zpviqaCj7daDc58QK6AHoskuWMEnrsI2zXbJjCpxSiJp5eAtXHfkZT5n6JdEZrgWnke6+Bvtu43kLSLxhLG5fXNOgZhKpZLd5NOv2/tL1M8ir/OtFsWdAvtnE5zykK7EZ157/Pdv82afAaxcOwkjwHgNNTb4/TOI68GjPZubm3ue9Frzcdt3E6wb+XAduvW767dauDWuUqqQ==
```

Or it can look like this 

```
#cloud-config

hostname: rancher

rancher:
  network:
    interfaces:
      eth*:
       dhcp: true
      

ssh_authorized_keys:
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCw0246LpRtBBJV4t9gsPRaGy5ZMfJAZ63fHN7EdBzZvWO6jrS5BdRGujt50sykXo54mpkbrKCngWqz+q5glqbiyXBVhygB0K1IGILFuxKHiShBsynM/2q613VQeMDNqs6PCzToS8z2Dyz6Axo/45vIfRfd+ruhVaeC2J/bzoka3Uq1as5MwGNB+FKlirhqUDJHUFEE2cqFj8O2hnOj4KwtwrTfpc+689Uxg4MnlKdT6bgLKcp49vg9rmQoTkdWknxZ8UcSG9QR/Y4ObsFJicYJx8XIDJUBXvIoqJd0udW+TfOkIiKStUQy9AAbWRAjz3qbZXeDCASOJA5H8a8Nf98/
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDznoT7SGEe/Yld35/FmQF0UWDkn/k4zZLggisEbnjr8xgWW0pNMvwt7gH9GvCQtTb5jDj2VYlTah52/00E73mD28OfY/8L1Lu9bs3mtkh0jpeY2C7+qPWgVG0kVQE62InEsK5DfhjI6BUDajHVucvdNr0BERax8J9BCQM45uG62O2JqBkFGsvbJsleeAbuPavYIi9dAqs/A9OBjlib0r5ysXjIkC0YnG99T9bRsyuCdefvSD8pLr7aM0UPRpZZqxYmexr4WLocChqCwR9DO+GCnLwZep4VL2n/uK9BDe0awXhZ/MJDMhpFAZsSJZG5TQk7Ry5n7sfRUozsjW0D66If
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDKowVHY6Y5Ar+AYAUeCJiMHH+UQNUZjN8De5dc/GKZOROIKYRxHAaPIgtvoHNNSU7w30p3gCvJaeFrqXr/HXFWJOp+2lRE6QDWIM8WqAJNwL87es7OAEyx8H+WLxoLpp/sYLAwV5xe49RGDrMJUfW6Rw2mLDiojlddB+t4L+fugPAb5K2UlxOAQ8ZD56hzcDAWwlADJ2+cavWqRrlsANgKJWrLTE0N9N/lcr9l3ar4W/ZP1Dq16d6s3pO2JVwcDdtz0jcOBhPnCVpIJIt0oykUU2IwiF9LErLZdB4Qm+2H1CM3X/RP9cd8aEZ6gmMmkTu4mT5ge4yWm7w50vj7INgqkyCBwEPRk9d2Cmh9maDr5sU8FyLnD8MnvNu80LwqfTaIA9NAI6VpVAuvTjrLHVaV4+KG50NcnG9Mw6Z8O6T8Cx7k1bDAXelarpA6fGNFkXkECC2mFdDvPVqRZmVvij/b5qEYToSOfsJytOLMFPZCnUQ/CRWsZMRyMSrokNQ10AmScN9rhAErbx4/c+utfEmtEpNBm2t9z2GiVvkLyGycN09gnVmlNkgv5xojAO4HPKdcfAml00lzu8ydtYhSyo7q91XImRbZSlyB360K7JZ0qTCIzPObqbQGwCMQOZ2Tt9+plqLuCQfdQiF0w9xOiufDCs+zdDO3FMuxg3e6c9mmew==
  - ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCtrbOooF4bEBdymEB9N5jh2mDCLAmWO5TCh0gorNOWyC5htBAJ0gqHGdh6OHqg2wT5sLDSMQ/FrpaOIdW5ULitN3aL+LTNDEhrU9mHTsPTlU2Bh7Fl6pnanQp7peUiEzbi7W7jRaC/IwDQwZh1tvSX2sUOVk+0nz7MtJXiyp3gfexltvySrSCHPRe4zZRrMu9PZ/LiaSJ0NTyCP/xEDYhXgV3g0R6YqRxiaXBzi/8Y4EgAYx/upekvB8OnVxYKXt6B+2Gqx4K1/GribiqL6ymevDUja0DIXumD6POqohbtjB3b+yka2pFoydnB0hkquEo7J9aB41DIszyePjci3Ur3ncbWJkS1Z5nkaAlVCziO8jLbBaQnVxVjE7QIXvqGePQRsMV3u6E5jGeJDq1Ub8Ynm91aX6iwrj3POhGIlxcJMMQkxc+2o+I23xNY22aoUXt5j9B543U2Ygsq3I3JPtNLFJbnIFQQv0zpviqaCj7daDc58QK6AHoskuWMEnrsI2zXbJjCpxSiJp5eAtXHfkZT5n6JdEZrgWnke6+Bvtu43kLSLxhLG5fXNOgZhKpZLd5NOv2/tL1M8ir/OtFsWdAvtnE5zykK7EZ157/Pdv82afAaxcOwkjwHgNNTb4/TOI68GjPZubm3ue9Frzcdt3E6wb+XAduvW767dauDWuUqqQ==

```
 Add a console in the cloud-config.

```
#cloud-config
rancher:
  console: debian
```
The command to validate the config 

`sudo ros install -c cloud-config.yml`

If you want to install then

`sudo ros install -c cloud-config.yml -d /dev/sda`

Listing Available Consoles
```
sudo ros console list
disabled alpine
disabled centos
disabled debian
current  default
disabled fedora
disabled ubuntu

```

The way to change to a more permanent console that does not change every reboot like the default busybox console.

`sudo ros console switch ubuntu`


Console persistence

All consoles except the default (busybox) console are persistent. Persistent console means that the console container will remain the same and preserves changes made to its filesystem across reboots. If a container is deleted/rebuilt, state in the console will be lost except what is in the persisted directories.

```
/home
/opt
/var/lib/docker
/var/lib/rancher

```

Link:

[rancherOS-custom-console](https://rancher.com/docs/os/v1.x/en/installation/custom-builds/custom-console/#console-persistence)


[RancherOS installing to hard disk - Automated Ramblings](https://sdbrett.com/BrettsITBlog/2017/01/rancheros-installing-to-hard-disk/)





