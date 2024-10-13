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

Here, we are creating a single partition as explained [on Cubiekube install procedure](https://archlinuxarm.org/platforms/armv7/allwinner/cubieboard-2). Here the result with my 32Gb card.

```
# fdisk -l                                                        
Disk /dev/mmcblk0: 30.23 GiB, 32462864384 bytes, 63404032 sectors               
Units: sectors of 1 * 512 = 512 bytes                                           
Sector size (logical/physical): 512 bytes / 512 bytes                           
I/O size (minimum/optimal): 512 bytes / 512 bytes                               
Disklabel type: dos                                                             
Disk identifier: 0x0de60300                                                     
                                                                                
Device         Boot Start      End  Sectors  Size Id Type                       
/dev/mmcblk0p1       2048 63404031 63401984 30.2G 83 Linux     
```

Follow steps **#3** to **#6** (creating and populating the filesystem)

# install u-boot

Install the spl bin file provided in this package.

`dd if=u-boot-sunxi-with-spl.bin of=/dev/sdb bs=1024 seek=8`

# Copy boot.scr

`cp boot.scr /mnt/boot/`

# Finishing

Unmount the SDCard :

```
# umount /mnt
# sync
```

Put the card on the PI ... you can boot your Banana.

# Securing root

- Login as the default user **alarm** with the password *alarm*.
- The default **root** password is *root*.

## Initializing pacman

```
# pacman-key --init
# pacman-key --populate archlinuxarm
```

## Time to update the system

`# pacman -Syu`

## Install sudo

`# pacman -S sudo`

and allow **alarm** to the group *wheel*.

`gpasswd -a alarm wheel`

and allow wheel in sudo list using `visudo` by uncomment the following line

`wheel ALL=(ALL:ALL) ALL`
