---
layout: post
title: MySQL无法重启问题解决Warning: World-writable config file ‘/etc/my.cnf’ is ignored
date: 2015-03-30 16:51
author: admin
comments: true
categories: []
---
今天帮朋友维护服务器，在关闭数据库的命令发现mysql关不了，提示Warning: World-writable config file ‘/etc/my.cnf’ is ignored ，大概意思是权限全局可写，任何一个用户都可以写。mysql担心这种文件被其他用户恶意修改，所以忽略掉这个配置文件。这样mysql无法关闭。

下面看下整个过程

重启MySQL

[root@ttlsa ~]
# service mysqld stop
Warning: World-writable config file '/etc/my.cnf' is ignored
Warning: World-writable config file '/etc/my.cnf' is ignored
MySQL manager or server PID file could not be found![FAILED]
可以看到mysql停止不了

查看my.cnf的权限

[root@ttlsa ~]
# ls -l /etc/my.cnf
-rwxrwxrwx 1 root root 4878 Jul 30 11:31 /etc/my.cnf
权限777，任何一个用户都可以改my.cnf，存在很大的安全隐患.

修复MySQL问题

[root@ttlsa ~]
# chmod 644 /etc/my.cnf
my.cnf设置为用户可读写，其他用户不可写.

关闭MySQL

[root@ttlsa ~]
# service mysqld stop
Shutting down MySQL..[  OK  ]
MySQL关闭成功. 问题很简单
