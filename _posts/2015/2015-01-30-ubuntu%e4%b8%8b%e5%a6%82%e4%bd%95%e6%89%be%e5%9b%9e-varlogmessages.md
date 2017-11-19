---
layout: post
title: Ubuntu下如何找回 /var/log/messages
date: 2015-01-30 10:23
author: admin
comments: true
categories: []
---
当我执行 sudo tail /var/log/messages时候，没有任何输出，在一看发现 /var/log/messages没有内容，找了一下才发现，Ubuntu已经将默认的系统日志记录在了 /var/log/syslog 文件中，加入我们想要找回原来的 /var/log/messages。由于这些日志文件时候 rsyslog服务维护的，我们可以通过修改 syslog的配置文件使其将日志记录在 /var/log/messages.
/etc/rsyslog.d/*.conf 这些配置文件被 /etc/rsyslog.conf 通过 $IncludeConfig 指令包含进来。
我么可以通过修改 /etc/rsyslog.d/50-default.conf 配置文件实现
去掉注释或添加：
*.=info;*.=notice;*.=warn;\
auth,authpriv.none;\
cron,daemon.none;\
mail,news.none -/var/log/message

然后重启rsyslog服务
sudo service rsyslog restart
