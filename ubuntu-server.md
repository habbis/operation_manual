# Ubuntu-server 



Turn on unattended security
dpkg-reconfigure -plow unattended-upgrades

## Install NVIDIA Graphic Driver


show drivers 

```
sudo Ubuntu-drivers devices
```

install drivers
```
sudo ubuntu-drivers autoinstall
```



  	
If your Computer has NVIDIA Graphic cards, Install NVIDIA Graphic Driver to improve Graphics related performance.
[1] 	Disable [nouveau] driver that is loaded by default as generic graphic driver. 

```
root@dlp:~# lsmod | grep nouveau

nouveau              1949696  0
mxm_wmi                16384  1 nouveau
wmi                    32768  2 mxm_wmi,nouveau
video                  49152  1 nouveau
i2c_algo_bit           16384  1 nouveau
ttm                   106496  1 nouveau
drm_kms_helper        184320  1 nouveau
drm                   491520  4 drm_kms_helper,ttm,nouveau
```
root@dlp:~# vi /etc/modprobe.d/blacklist-nouveau.conf
```
# add to the end (create new if it does not exist)

blacklist nouveau
options nouveau modeset=0
root@dlp:~# update-initramfs -u
root@dlp:~# reboot
[2] 	NVIDIA Driver is provided on Ubuntu official repository.
Before installing, make sure the Driver Version for your Graphic Card on NVIDIA official site below.
⇒ https://www.nvidia.com/Download/index.aspx?lang=en
# confirm the cards on your computer
```

root@dlp:~# lspci | grep VGA
```
04:00.0 VGA compatible controller: NVIDIA Corporation GP104 [GeForce GTX 1070] (rev a1)
# install the driver for your card

root@dlp:~# apt -y install nvidia-driver-440
# verify installation to show Graphic cards' status

root@dlp:~# nvidia-smi
```
```
Fri Jul 24 19:15:19 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 440.100      Driver Version: 440.100      CUDA Version: 10.2     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1070    Off  | 00000000:04:00.0 Off |                  N/A |
| 27%   37C    P0    37W / 180W |      0MiB /  8119MiB |      2%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

links: 

https://www.server-world.info/en/note?os=Ubuntu_20.04&p=nvidia&f=1
