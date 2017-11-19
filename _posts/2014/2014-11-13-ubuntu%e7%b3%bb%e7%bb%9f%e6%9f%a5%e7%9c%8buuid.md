---
layout: post
title: Ubuntu系统查看UUID
date: 2014-11-13 14:05
author: admin
comments: true
categories: []
---
查看UUID:

代码:
sudo blkid

代码:
less /etc/fstab

代码:
ls -al /dev/disk/by-uuid

硬盘分区标签LABEL的指定：
　 设定swap分区的LABEL:

代码:
mkswap -L LABEL名称 /dev/分区名称

　　挂载fstab中的swap：

代码:
swapon -a

　　设定普通分区的LABEL：

代码:
tune2fs -L LABEL名称 /dev/分区名称

代码:
sudo e2label /dev/分区名称 名称 例如：sudo e2label /dev/sda1 boot

　　查看普通分区的LABEL：
 
代码:
tune2fs -l /dev/分区名称



盘符挂载NTFS分区 分区名为英文

代码:
sudo apt-get install ntfs-config

代码:
sudo gedit /etc/fstab
