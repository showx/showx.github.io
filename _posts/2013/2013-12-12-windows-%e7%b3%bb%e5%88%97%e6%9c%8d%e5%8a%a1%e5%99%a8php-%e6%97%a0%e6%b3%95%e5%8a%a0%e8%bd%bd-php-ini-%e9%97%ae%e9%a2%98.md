---
layout: post
title:  windows 系列服务器PHP 无法加载 php.ini 问题 
date: 2013-12-12 14:18
author: admin
comments: true
categories: []
---
以前在WIN下配置apache+php+mysql的時候，是把php.ini以及libmysql.dll等文件copy到windows下的，但是一但重装系统的时候，忘记备份的话，还得重新配置php.ini，只不过后来也没有时间去研究是不是可以不放在windows文件夹下。

现在换了份工作，然后用前人用的机器，发现他们装的PHP+MYSQL是不用把php.ini放在windows下的，于是google了一下，我也能配置不用把php.ini放在windows文件夹下了。

 

有两个方法，1是设置系统环境变量；2是在apache的http.conf里设置php.ini的位置。具体：

 

方法1、参考：http://www.cnblogs.com/leilei/archive/2008/06/12/1218613.html

①将 PHP 目录加入到 Windows 路径 PATH 中去

在 Windows NT，2000，XP 和 2003 下：

进入控制面板并打开“系统”图标（开始 -> 设置 -> 控制面板 -> 系统，Windows XP/2003 中是：开始 -> 控制面板 -> l系统）
选择“高级”标签页
点击“环境变量”按钮
在“系统变量”栏中
找到 Path 这一项（可能需要向下滚动才能找到）
鼠标双击 Path 这一项
在最后加入你的 PHP 目录，包括前面的英文分号“;”（例如：;C:/php ，我的路径是 ;E:/usr/php ）
点击“确定”并重新启动电脑
②使 php.ini 文件在 Windows 下被 PHP 所用

（这一步很重要，我实验了的，如果没有这一步，PHP 将搜寻不到 php.ini ）

在 Windows NT，2000，XP 和 2003 种：

进入控制面板并打开“系统”图标（开始 -> 设置 -> 控制面板 -> 系统，Windows XP/2003 中是：开始 -> 控制面板 -> l系统）
选择“高级”标签页
点击“环境变量”按钮
在“系统变量”栏中
点击“新建”按钮并在“变量名”中输入“PHPRC”，在“变量值”中输入 php.ini 文件所在的目录（例如：C:/php）
点击“确定”并重新启动电脑
方法2、参考：http://topic.csdn.net/u/20081216/16/2C9D3EA2-567E-4031-9F8D-193F493B343A.html

在http.conf中加入：
PhpIniDir "E:/httpd/php"
LoadFile "E:/httpd/php/libmysql.dll"
LoadFile "E:/httpd/php/libmcrypt.dll"

（路径换为自己的）

 

比如：

httpd.conf:

LoadModule php5_module "D:/php_server/php/php5apache2_2.dll"
AddType application/x-httpd-php .php

PhpIniDir "D:/php_server/php"
LoadFile "D:/php_server/php/libmysql.dll"
LoadFile "D:/php_server/php/libmcrypt.dll"

 

php.ini:

extension_dir = "D:/php_server/php/ext/"
