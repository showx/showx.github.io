---
layout: post
title: linux init命令详解
date: 2015-02-02 18:10
author: admin
comments: true
categories: []
---
<strong>linux</strong> <a name="baidusnap1"></a><strong>init</strong>命令详解（转）
<div></div>
<div id="blog_text">
<strong>Init</strong>是<strong>Linux</strong>操作系统中不可缺少的程序之一。<strong>init</strong>进程是<strong>Linux</strong>内核引导运行的，是系统中的第一个进程，其进程号（PID）永远为1。你可以通过#ps   -ef|head来查看进程命令。

1)     几个常用的命令
查看系统进程命令：#ps   -ef|head
查看<strong>init</strong>的配置文件：#more   /etc/inittab
查看系统当前运行的级别：#runlevel
2)   运行级别
到底什么是运行级呢？简单的说，运行级就是操作系统当前正在运行的功能级别。这个级别从0到6 ，具有不同的功能。你也可以在/etc/inittab中查看它的英文介绍。
#0                     停机(千万不能把initdefault 设置为0)
#1                     单用户模式
#2                     多用户，没有 NFS(和级别3相似，会停止部分服务)
#3                     完全多用户模式
#4                     没有用到
#5                     x11(Xwindow)
#6                     重新启动(千万不要把initdefault 设置为6)</div>
&nbsp;
