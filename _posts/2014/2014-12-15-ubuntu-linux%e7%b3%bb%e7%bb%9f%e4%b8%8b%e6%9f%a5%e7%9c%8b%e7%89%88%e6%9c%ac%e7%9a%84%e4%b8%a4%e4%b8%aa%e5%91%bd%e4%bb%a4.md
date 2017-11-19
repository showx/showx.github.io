---
layout: post
title: Ubuntu Linux系统下查看版本的两个命令
date: 2014-12-15 17:58
author: admin
comments: true
categories: []
---

方法一：

在终端中执行下列指令：

cat /etc/issue

可以查看当前正在运行的 Ubuntu 的版本号。其输出结果类似下面的内容：

Ubuntu 7.04 n l

方法二：

使用 lsb_release 命令也可以查看 Ubuntu 的版本号，与方法一相比，内容更为详细。执行指令如下：

sudo lsb_release -a

将输出结果：

Distributor ID: Ubuntu

Description: Ubuntu 7.04

Release: 7.04

Codename: feisty
