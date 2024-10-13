ArchLinux BananaPI support files
---

This repository contains **u-boot**  binary and boot script suitable to run **ArchARM** on a BananaPI M1.

The procedure is the following :

# identify your SD card.

```
# lsblk
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda  	8:0	0 465,8G  0 disk
├─sda1   8:1	0   512M  0 part /boot
└─sda2   8:2	0 465,3G  0 part /
sdb  	8:16   1 	0B  0 disk
sr0 	11:0	1  1024M  0 rom  
zram0  254:0	0   3,8G  0 disk [SWAP]
```

Here it's **/dev/sdb**. 

> [!CAUTION]
> Double check, triple check : wrong identification may destroy your host's system.

# Erase the beginning of the SD card

`dd if=/dev/zero of=/dev/sdb bs=1M count=8`

# Partition your SDCard

Here, we are creating a single partition.

