---
layout: post
title:  fdisk 格式化U盘,用于安装ubuntu linux操作系统 
date: 2014-12-16 10:07
author: admin
comments: true
categories: []
---


查看U盘设备,sdb,sdb1就位于U盘上:
yq@bear:~$ ls /dev/sd*
/dev/sda  /dev/sda1  /dev/sda2  /dev/sda3  /dev/sda4  /dev/sda5  /dev/sda6  /dev/sda7  /dev/sda8  /dev/sdb  /dev/sdb1
yq@bear:~$ 
yq@bear:~$ sudo fdisk /dev/sdb
[sudo] password for yq: 

Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)


Command (m for help): p


Disk /dev/sdb: 7747 MB, 7747397632 bytes
32 heads, 63 sectors/track, 7505 cylinders, total 15131636 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0007b467


   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1   *          63    15130079     7565008+   b  W95 FAT32



Command (m for help): p


Disk /dev/sdb: 7747 MB, 7747397632 bytes
32 heads, 63 sectors/track, 7505 cylinders, total 15131636 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0007b467


   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1   *          63    15130079     7565008+   b  W95 FAT32


Command (m for help): d
Selected partition 1


Command (m for help): p


Disk /dev/sdb: 7747 MB, 7747397632 bytes
32 heads, 63 sectors/track, 7505 cylinders, total 15131636 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0007b467


   Device Boot      Start         End      Blocks   Id  System


Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 
Using default value 1
First sector (2048-15131635, default 2048): 
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-15131635, default 15131635): 13034483


Command (m for help): p


Disk /dev/sdb: 7747 MB, 7747397632 bytes
32 heads, 63 sectors/track, 7505 cylinders, total 15131636 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0007b467


   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    13034483     6516218   83  Linux


Command (m for help): n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): e
Partition number (1-4, default 2): 
Using default value 2
First sector (13034484-15131635, default 13034484): 
Using default value 13034484
Last sector, +sectors or +size{K,M,G} (13034484-15131635, default 15131635): 
Using default value 15131635


Command (m for help): p


Disk /dev/sdb: 7747 MB, 7747397632 bytes
32 heads, 63 sectors/track, 7505 cylinders, total 15131636 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0007b467


   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    13034483     6516218   83  Linux
/dev/sdb2        13034484    15131635     1048576    5  Extended


Command (m for help): n
Partition type:
   p   primary (1 primary, 1 extended, 2 free)
   l   logical (numbered from 5)
Select (default p): l
Adding logical partition 5
First sector (13036532-15131635, default 13036532): 
Using default value 13036532
Last sector, +sectors or +size{K,M,G} (13036532-15131635, default 15131635): 
Using default value 15131635


Command (m for help): p


Disk /dev/sdb: 7747 MB, 7747397632 bytes
32 heads, 63 sectors/track, 7505 cylinders, total 15131636 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0007b467


   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    13034483     6516218   83  Linux
/dev/sdb2        13034484    15131635     1048576    5  Extended
/dev/sdb5        13036532    15131635     1047552   83  Linux


Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)


Command (m for help): t
Partition number (1-5): 5
Hex code (type L to list codes): 82
Changed system type of partition 5 to 82 (Linux swap / Solaris)


Command (m for help): p


Disk /dev/sdb: 7747 MB, 7747397632 bytes
32 heads, 63 sectors/track, 7505 cylinders, total 15131636 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0007b467


   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    13034483     6516218   83  Linux
/dev/sdb2        13034484    15131635     1048576    5  Extended
/dev/sdb5        13036532    15131635     1047552   82  Linux swap / Solaris


Command (m for help): w
The partition table has been altered!


Calling ioctl() to re-read partition table.

查看结果:
yq@bear:~$ sudo fdisk  -l


// sda 硬盘部分省略

Disk /dev/sdb: 7747 MB, 7747397632 bytes
239 heads, 62 sectors/track, 1021 cylinders, total 15131636 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x0007b467


   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    13034483     6516218   83  Linux
/dev/sdb2        13034484    15131635     1048576    5  Extended
/dev/sdb5        13036532    15131635     1047552   82  Linux swap / Solaris
yq@bear:~$ 


格式化:
umount /dev/sdb1 并重新插拔一下U盘,
sudo mkfs.ext4 /dev/sdb1

sudo mkswap /dev/sdb5

下面就可以用UnetBootin在U盘上安装操作系统了,当然,也可以不用fdisk分区,在安装的时候图形化分区.  这里就是记录一下fdisk的使用方法而已. 
