---
layout: post
title: Navicat for MySQL 11 Linux 破解方法
date: 2015-07-02 09:14
author: admin
comments: true
categories: []
---
安装：解压后即可用。目录下的start_navicat文件为可执行文件。

破解：(找过好几个注册码都不能用，注册码生成器都是Windows平台的，Linux下不行)

—-第一次执行start_navicat时，会在用户主目录下生成一个名为.navicat的隐藏文件夹。

—-把此文件夹删除后(删除文件夹命令是<code>rm -rf .navicat</code>)，下次启动navicat 会重新生成此文件夹，30天试用期会按新的时间开始计算。

&nbsp;

======

破解方法一、
navicat linux版本有一个月的试用期， 当过了试用期以后， 不能再进入。
但其实只要将~下.navicat目录下的system.reg文件删掉， 重新启动navicat的时候， 就可以再获得一个月的试用期，而且前面使用的连接配置都还在。

&nbsp;
