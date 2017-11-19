---
layout: post
title: Can't find file: './mysql/plugin.frm' (errno: 13)[mysql数据目录迁移错位]错误解决 - 北风飘
date: 2015-07-21 11:02
author: admin
comments: true
categories: []
---
大概需要4个步骤，其中第1步通过service mysql stop停止数据库，第4步通过service mysql start启动数据库。

第2步移动数据文件，不知道是否为Ubuntu智能的原因，移动数据库的时候，除了数据文件，连权限也一起带过去了 
root@T60:~#mv /var/lib/mysql /home/ 
我还在记录/var/lib/mysql各目录的权限，当mv完成之后，/home/下面的权限保留原来/var/lib/mysql的各类权限，其中有目录，文件等等，连chown,chmod都给省事了。

本想这样就没事了，执行第4步一直启动不了，看日志文件/var/log/mysql/error.log的结果是

/usr/sbin/mysqld: Can't find file: './mysql/plugin.frm' (errno: 13)100803

12:36:36 [ERROR] Can't open the mysql.plugin table.

Please run mysql_upgrade to create it.100803 12:36:36

InnoDB: Operating system error number 13 in a file operation.

InnoDB: The error means mysqld does not have the access rights to

InnoDB: the directory.

InnoDB: File name ./ibdata1InnoDB: File operation call: 'open'.

InnoDB: Cannot continue operation.

刚才mv过去的权限都是对的，不存在还有什么权限的问题，到网上搜索大部分都是redhat系列的系统，其中的SElinux对ubuntu不适用，还差点真用chcon来修改所谓的安全之类的权限了

其实，在my.cnf中注释部分说明的很清楚

# * IMPORTANT# If you make changes to these settings and your system uses apparmor, you may# also need to also adjust /etc/apparmor.d/usr.sbin.mysqld.

按照说明进行第3步，注释掉原来的路径，增加新的路径，2行都要修改

root@T60:~# vi /etc/apparmor.d/usr.sbin.mysqld

# /var/lib/mysql/ r,# /var/lib/mysql/** rwk, 改为 /home/mysql/ r, /home/mysql/** rwk,

更新参数

root@T60:~# /etc/init.d/apparmor reload

* Reloading AppArmor profiles Skipping profile in /etc/apparmor.d/disable: usr.bin.firefox [ OK ]

修改完成后执行第4步root@T60:~# service mysql start 
可以在日志中看见启动成功 
/var/log/mysql/error.log 
100803 12:53:07 InnoDB: Started; log sequence number 0 5646278 
100803 12:53:07 [Note] Event Scheduler: Loaded 0 events 
100803 12:53:07 [Note] /usr/sbin/mysqld: ready for connections. 
Version: ’5.1.41-3ubuntu12.4-log’ socket: ‘/var/run/mysqld/mysqld.sock’ port: 3306 (Ubuntu)
