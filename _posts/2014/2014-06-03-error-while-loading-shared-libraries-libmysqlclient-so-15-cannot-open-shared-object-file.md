---
layout: post
title: error while loading shared libraries: libmysqlclient.so.15: cannot open shared object file  
date: 2014-06-03 09:59
author: admin
comments: true
categories: []
---
先 libmysqlclient.so.15确实存在，出现这种错误可能是没有装载上。通过下列方法解决。
locate libmysqlclient.so.15
没有的话请下载一个
wget http://files.directadmin.com/services/es_5.0_64/libmysqlclient.so.15
cp libmysqlclient.so.15  /usr/lib/

执行ldconfig -v  更新配置。
