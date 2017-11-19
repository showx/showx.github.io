---
layout: post
title: connect() failed (111: Connection refused) while connecting to upstream解决
date: 2015-10-22 19:26
author: admin
comments: true
categories: []
---
有时候nginx运行很正常，但是会发现错误日志中依旧有报错connect() failed (111: Connection refused) while connecting to upstream.
一般情况下我们的upstream都是fastcgi://127.0.0.1:9000. 造成这个问题的原因大致有两个
 php-fpm没有运行
执行如下命令查看是否启动了php-fpm，如果没有则启动你的php-fpm即可

netstat -ant | grep 9000
1
netstat -ant | grep 9000
 
php-fpm队列满了
php-fpm.conf配置文件pm.max_children修改大一点,重启php-fpm并观察日志情况
