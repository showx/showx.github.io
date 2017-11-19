---
layout: post
title: 使用网易镜像给Ubuntu的apt-get提速(sohu那个镜像，电信线路太慢了)
date: 2014-12-15 17:45
author: admin
comments: true
categories: []
---
刚安装的ubuntu系统, 安装的中文版12.04.2， apt-get会自动使用 sohu的（cn.archive.ubuntu.com、mirrors.sohu.com）。   电信线路真的受不了的。 sohu很有可能是用的网通的，    网易的（mirrors.163.com）就不知道了，总之非常的快。

修改/etc/apt/sources.list

?
1
sudo gedit /etc/apt/sources.list
编辑/etc/apt/sources.list文件, 在文件最前面添加以下条目(操作前请做好相应备份)


?
1
2
3
4
5
6
7
8
9
10
deb http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ precise-proposed main restricted universe multiverse
    deb http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise-security main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise-updates main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise-proposed main restricted universe multiverse
    deb-src http://mirrors.163.com/ubuntu/ precise-backports main restricted universe multiverse

参考：http://mirrors.163.com/.help/ubuntu.html

修改之后， 执行

?
1
2
sudo apt-get update
sudo apt-get upgrade
享受丁总提供的福利把。 ：）
