---
layout: post
title: Win7，Win8 nginx默认80端口被System占用，造成nginx启动报错的解决方案
date: 2014-12-29 11:23
author: admin
comments: true
categories: []
---
Win7下nginx默认80端口被System占用，造成nginx启动报错的解决方案
 
在win7 32位旗舰版下，启动1.0.8版本nginx，显示如下错误：
 
[plain]
2012/04/02 13:55:59 [emerg] 7864#2376: bind() to 0.0.0.0:80 failed (10013: An attempt was made to access a socket in a way forbidden by its access permissions)  
 
在cmd窗口运行如下命令：
 
[plain]
C:\Users\Administrator>netstat -aon | findstr :80  
  www.2cto.com  
看到80端口果真被占用。发现占用的pid是4，名字是System。怎么禁用呢？
 
1、打开注册表：regedit
 
2、找到：HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\HTTP
 
3、找到一个REG_DWORD类型的项Start，将其改为0
 
4、重启系统，System进程不会占用80端口
 
重启之后，start nginx.exe 。在浏览器中，输入127.0.01，即可看到亲爱的“Welcome to nginx!” 了。
 
