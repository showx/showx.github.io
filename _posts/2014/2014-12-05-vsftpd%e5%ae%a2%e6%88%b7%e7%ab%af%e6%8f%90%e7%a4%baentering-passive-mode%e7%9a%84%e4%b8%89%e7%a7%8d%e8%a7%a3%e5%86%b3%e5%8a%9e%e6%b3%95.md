---
layout: post
title: vsftpd客户端提示:Entering passive Mode的三种解决办法
date: 2014-12-05 13:51
author: admin
comments: true
categories: []
---
一般情况上，如下这个提示Entering Passive mode，要求输入被动模式，输完后，发现仍然没有反应。
解决方法：通常，第一条不一定有效果。第二条，第三条效果显著
方法一：修改vsftpd.conf,添加一条pasv_promiscuous=YES 关闭passive的安全检测。
方法二：修改vsftpd.conf文件，添加: <以被动模式启动，开放被动的访问端口>
pasv_min_port=30000
pasv_max_port=30999
然后定义iptables规则：
iptables –A INPUT –p tcp ——dport 30000:30999 –j ACCEPT
iptables –A INPUT –p tcp ——dport 30000:30999 –j ACCEPT
iptables –A INPUT –p tcp ——dport 21 –j ACCEPT
方法三：将vsftpd的模式修改为主动模式：<以主动模式起动>
pasv_enable=no 

==================
用useradd命令添加帐号test，提示“user test exists" ,但是 在root下用
su test 时，提示This account is currently not available，我该如何使这个帐号能用呢，我不想删了重建，请高手指教。


用vipw看看test的帐号信息，如果它的shell是/sbin/nologin,
把它改成正常允许的shell试一下。

=================
如果列出慢，要看防火墙相关的了。
