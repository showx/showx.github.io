---
layout: post
title: 升级yosemite 之后mysql使用不了
date: 2014-10-30 14:50
author: admin
comments: true
categories: []
---
升级yosemite 之后mysql使用不了

ps -feax | grep mysqld 
And verifying that you get something similar to this:

    0  8308     1   0   0:00.08 ??         0:00.11 /bin/sh /usr/local/mysql/bin/mysqld_safe
or looking for a mysqld process on the Activity Monitor application

If it's not running you have to start the daemon.

Start MySQL daemon
On Mac OSX you can start the daemon from the Terminal Application with something like this:

sudo /usr/local/mysql/bin/mysqld_safe &

可以使用mysqld_safe 启动试度，加进守护进程那里
