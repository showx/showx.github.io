---
layout: post
title: mysql ERROR 1018 (HY000): Can't read dir of '.' (errno: 24)
date: 2015-07-21 11:08
author: admin
comments: true
categories: []
---
网上查找 (errno: 24) 是由于进程打开文件数过多引起的。
可以试着关闭mysql进程。然后重新启动。
kill -9 mysql的进程号     --可能有多个，都关闭掉（不知道关闭的顺序有无影响，如果不行，调整关闭顺序）
然后重新启动
/etc/init.d/mysql start
ps：
查看mysql进程打开的文件数
lsof -u mysql

