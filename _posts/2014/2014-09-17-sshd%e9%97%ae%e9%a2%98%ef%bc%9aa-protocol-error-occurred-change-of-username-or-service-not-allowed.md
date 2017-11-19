---
layout: post
title: sshd问题：A protocol error occurred. Change of username or service not allowed 
date: 2014-09-17 16:20
author: admin
comments: true
categories: []
---
sshd问题：A protocol error occurred. Change of username or service not allowed
在研究linux安全的时候遇到一个问题
原本打算修改linux直接远程root登陆，修改为sshd的配置文件后
Nano /etc/ssh/sshd_config
把#PermitRootLogin yes
修改为PermitRootLogin no
修改完成后，保存退出
重启sshd
service sshd restart
新建一个普通用户
Useradd unixbar Passwd unixbar
在securecrt远程工具中，使用普通用户登陆的时候，出现了
The server has disconnected with an error. Server message reads: A protocol error occurred. Change of username or service not allowed: (shang1,ssh-connection) -> (shang,ssh-connection)
这样类似的错误。
这是由于securecrt设置的原因导致的。把你的用户名改成unixbar。就可以了，这样做了以后就可以实现禁止root登录
secureccrt->session options->ssh2->username填写正确就行了。
