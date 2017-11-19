---
layout: post
title: 解决vsftpd的530 Login incorrect错误
date: 2015-01-12 22:49
author: admin
comments: true
categories: []
---
530 Login incorrect
只有用匿名anonymous才可登录,其余所有用户都报530 Login incorrect错
local_enable=YES
write_enable=YES
pam_service_name=vsftpd
userlist_enable=YES
加入这句话就OK啦.现在原因还不知道.
 
其他的解决思路：1、被动模式的问题
2、有时候可能是主目录的问题，比如你的FTP主目录是/data/www，但是用户vsftpd的在/etc/passwd不是这个目录也会出问题，记住查看日志。
 
（我的FTP报这个错是因为我把/etct/passwd下FTP用户的/sbin/nologin改成了/bin/bash,改回原来的禁止登录服务器就OK ）
