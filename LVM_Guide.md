# LVM

Definitions

    PV : Physical Volumes. This means the hard disk, hard disk partitions, RAID or LUNs from a SAN which form "Physical Volumes" (or PVs).  

    VG : Volume Groups. This is a collection of one or more Physical Volumes.

    LV : Logical Volumes. LVs sit inside a Volume Group and form, in effect, a virtual partition.

    PE : Physical Extents. In order to manipulate the actual data, it is divided into blocks of data called Physical Extents.

    LE : Logical Extents. Similar to Physical Extents, but at the Logical Volume level. Physical Extents are to Physical Volumes as Logical Extents are to Logical Volumes. The size of blocks are the same for each logical volume (LV) in the same volume group (VG). 


To create PV


```
$ pvcreate /dev/sda2
```

list pv
```
$ pvs
```

Extend a volume group

Declare another physical volume:
```
$  pvcreate /dev/sda3
```

Then add the new PV to the VG that already exists:
```
$ vgextend myVirtualGroup1 /dev/sda3
```
Verify VG configuration

Simply run this command:
```
$ vgdisplay 
```
or
```
$ vgs
```

Create a volume group of physical volume
```
$ vgcreate myVirtualGroup1 /dev/sda2
```
List lv
```
$ lvs
```
Create a logical volume in a volume group:
```
$  lvcreate -n myLogicalVolume1 -L 10g myVirtualGroup1
```
Format the logical volume to the filesystem you want (ext4,xfs...)
```
$ mkfs -t ext4 /dev/myVirtualGroup1/myLogicalVolume1
```

To resize lvm volume 
```
$ lvresize -rL  +10G /path/to/lvolume
```
To reduce 
```
$ lvresize -rL  -10G /path/to/lvolume
```

Remove a LV

To remove a logical volume, make sure it is not in use anymore. Then simply issue this command to remove the logical volume myLogicalVolume1 in volume group myVirtualGroup1:
```
$ lvremove myVirtualGroup1/myLogicalVolume1
```

Thin lvm pool

vgcreate 

`lvcreate -L 15G --thinpool poolname vg-name`

Creating Thin Volumes

`lvcreate -V 5G --thin -n yourvolname vg-name/poolname`

List of PV commands

•     pvchange — Change attributes of a Physical Volume.

•     pvck — Check Physical Volume metadata.

•     pvcreate — Initialize a disk or partition for use by LVM.

•     pvdisplay — Display attributes of a Physical Volume.

•     pvmove — Move Physical Extents.

•     pvremove — Remove a Physical Volume.

•     pvresize — Resize a disk or partition in use by LVM2.

•     pvs — Report information about Physical Volumes.

•     pvscan — Scan all disks for Physical Volumes. 


List of VG commands

•     vgcfgbackup — Backup Volume Group descriptor area.

•     vgcfgrestore — Restore Volume Group descriptor area.

•     vgchange — Change attributes of a Volume Group.

•     vgck — Check Volume Group metadata.

•     vgconvert — Convert Volume Group metadata format.

•     vgcreate — Create a Volume Group.

•     vgdisplay — Display attributes of Volume Groups.

•     vgexport — Make volume Groups unknown to the system.

•     vgextend — Add Physical Volumes to a Volume Group.

•     vgimport — Make exported Volume Groups known to the system.

•     vgimportclone — Import and rename duplicated Volume Group (e.g. a hardware snapshot).

•     vgmerge — Merge two Volume Groups.

•     vgmknodes — Recreate Volume Group directory and Logical Volume special files

•     vgreduce — Reduce a Volume Group by removing one or more Physical Volumes.

•     vgremove — Remove a Volume Group.

•     vgrename — Rename a Volume Group.

•     vgs — Report information about Volume Groups.

•     vgscan — Scan all disks for Volume Groups and rebuild caches

•     vgsplit — Split a Volume Group into two, moving any logical volumes from one Volume Group to another by moving entire Physical Volumes. 




List of LV commands

•     lvchange — Change attributes of a Logical Volume.

•     lvconvert — Convert a Logical Volume from linear to mirror or snapshot.

•     lvcreate — Create a Logical Volume in an existing Volume Group.

•     lvdisplay — Display the attributes of a Logical Volume.

•     lvextend — Extend the size of a Logical Volume.

•     lvreduce — Reduce the size of a Logical Volume.

•     lvremove — Remove a Logical Volume.

•     lvrename — Rename a Logical Volume.

•     lvresize — Resize a Logical Volume.

•     lvs — Report information about Logical Volumes.

•     lvscan — Scan (all disks) for Logical Volumes. 





