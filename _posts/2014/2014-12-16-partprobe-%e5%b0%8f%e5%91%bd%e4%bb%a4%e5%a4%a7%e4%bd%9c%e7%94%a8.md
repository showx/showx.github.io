---
layout: post
title: partprobe 小命令大作用
date: 2014-12-16 10:00
author: admin
comments: true
categories: []
---
linux上，在安装系统之后，可否创建分区并且在不重新启动机器的情况下系统能够识别这些分区?
解决方法:

你可以使用一个叫做partprobe的工具。它包含在parted的rpm软件包中。在Red Hat Enterprise Linux 3上他的版本是parted-1.6。 partprobe 是一个可以修改kernel中分区表的工具。可以使kernel重新读取分区表。如下命令可以查看你的系统是否安装了parted软件包

rpm -q parted
举例来说：

# rpm -q parted
parted-1.6.3-29

你可以使用up2date命令安装这个软件包，如果在你的系统已经正确地注册到RHN上了。否则你可以从光盘上安装这个文件。

你可以使用fdisk或者其他命令创建一个新的分区，然后使用partprobe命令重新读取分区表。

# partprobe
这个命令执行完毕之后不会输出任何返回信息，你可以使用mke2fs命令在新的分区上创建文件系统。 


我用此命令解决 LVM  fdisk 划分分区问题~~　不需要硬盘umount
