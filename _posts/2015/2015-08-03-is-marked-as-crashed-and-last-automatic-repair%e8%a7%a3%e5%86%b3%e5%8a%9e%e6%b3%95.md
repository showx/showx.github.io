---
layout: post
title: is marked as crashed and last (automatic?) repair解决办法
date: 2015-08-03 09:34
author: admin
comments: true
categories: []
---
修复数据表操
MYSQL数据表出现问题，提示：
Error: Table './db_name/table_name' is marked as crashed and last (automatic?) repair failed
修复数据表操作：
1、service mysqld stop;
2、cd /var/lib/mysql/db_name/
3、myisamchk -r tablename.MYI (修复单张数据表)
myisamchk -r *.MYI (修复所有数据表)
注意：操作第三步前一定要把mysql服务停掉。
4. mysqlcheck -o -r 数据库名称 -p

＝＝＝＝＝＝＝＝＝＝
myisamchk -r mylog即可
