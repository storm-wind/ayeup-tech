# How to use LVM
LVM is used to manage disk capacity with Linux. 


---
***
### Expand an existing LVM

This assumes you have added an extra disk on VMware to the VM affected.

```
echo "- - -" > /sys/class/scsi_host/scan0
lsblk
fdisk /dev/sdd
pvcreate /dev/sdd1
vgs
vgextend centos /dev/sdd1
vgs
pvscan
lvextend -l +100%FREE /dev/centos/var
```

or for specifc sizes,

```
lvextend  -L <+size>   <Logical volume name path>

xfs_growfs /dev/mapper/centos-var
```

**Or if filesystem is not XFS then use this instead**
```
resize2fs /dev/mapper/centos-var
```
---
***

### Expand partition for active partition

How to expand a live partition.
This example uses /dev/sda2

```
fdisk /dev/sda
```

You will get something like this below,

```
Command (m for help): p

Disk /dev/sda: 68.7 GB, 68719476736 bytes, 134217728 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000bffd8

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     1050623      524288   83  Linux
/dev/sda2         1050624    36716543    17832960   8e  Linux LVM
```

So you have checked which one is which, now we delete the partition **/dev/sda2**.

```
Command (m for help): d
Partition number (1,2, default 2): 2
Partition 2 is deleted
```
Now we create a new partion with a new end block.

```
Command (m for help): n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): p
Partition number (2-4, default 2):
First sector (1050624-134217727, default 1050624): 1050624
Last sector, +sectors or +size{K,M,G} (1050624-134217727, default 134217727): 73433086
Partition 2 of type Linux and of size 34.5 GiB is set
```
Lets check what is now existing

```
Command (m for help): p

Disk /dev/sda: 68.7 GB, 68719476736 bytes, 134217728 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000bffd8

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     1050623      524288   83  Linux
/dev/sda2         1050624    73433086    36191231+  83  Linux
```

Also, change partition type, using option `t` and code `8e` to change to **Linux LVM**.

```
Command (m for help): wq
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.
```

Then run the following

```
partprobe
partx -u /dev/sda
cat /proc/partitions | grep sda
   8        0   67108864 sda
   8        1     524288 sda1
   8        2   36191231 sda2
```

Now run

```
pvresize /dev/sda2
```

You'll get this below

```
  Physical volume "/dev/sda2" changed
  1 physical volume(s) resized or updated / 0 physical volume(s) not resized
```

Now run

```
pvs
```

```
  PV         VG   Fmt  Attr PSize  PFree
  /dev/sda2  rhel lvm2 a--  34.51g 17.51g
```
---
## Parted

Select disk
```
parted
(parted) select /dev/sda
```

To see partitions
```
(parted) print
```

This defines START and END of disk locations.
```
(parted) mkpart primary 106 16179
```

### You can enable boot option on partition.
```
(parted) set 1 boot on
```

### Logical partition with 127GB 
```
(parted) mkpart logical 372737 500000
```

### Create a file system on Partition using mkfs
```
(parted) mkfs
```

### Create Parition and filesystem together using mkpartfs
```
(parted) mkpartfs logical fat32 372737 500000
```

### Resize partiiton from one size to another using resize
```
(parted) resize 9
```
---

