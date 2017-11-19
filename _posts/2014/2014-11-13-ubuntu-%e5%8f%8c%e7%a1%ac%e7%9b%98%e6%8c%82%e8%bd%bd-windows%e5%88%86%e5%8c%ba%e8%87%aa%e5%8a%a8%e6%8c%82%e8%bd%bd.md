---
layout: post
title: ubuntu 双硬盘挂载 windows分区自动挂载
date: 2014-11-13 10:49
author: admin
comments: true
categories: []
---

sudo fdisk -l
查看硬盘情况

1:新建一个目录,例:old

2:mount  /dev/sdb1  old

3:cd old

4:ls  (就可以看到新硬盘的内容了)

取消挂载:umount old

挂载虚拟卷的方法

1:sudo vgchange -ay /dev/ubuntu

2:sudo mount /dev/ubuntu/root old

