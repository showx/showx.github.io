---
layout: post
title: Linux/Mac OS 查看进程ps命令
date: 2015-05-30 01:02
author: admin
comments: true
categories: []
---
输入下面的ps命令，显示所有运行中的进程：

# ps aux | less
 

其中，

-A：显示所有进程

a：显示终端中包括其它用户的所有进程

x：显示无控制终端的进程

 

如何查看系统中每个进程？

ps -A
ps -e
 如何查看非root运行的进程？

ps -U root -u root
 如何查看具体某个用户运行的进程

ps -u user1
 top 命令提供了运行中系统动态的视图。

top

