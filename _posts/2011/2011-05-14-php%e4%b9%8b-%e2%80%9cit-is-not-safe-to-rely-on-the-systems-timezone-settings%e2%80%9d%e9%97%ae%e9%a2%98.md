---
layout: post
title: PHP之 “It is not safe to rely on the system's timezone settings”问题
date: 2011-05-14 01:50
author: admin
comments: true
categories: []
---
解决方法

   1：改 php.ini

[Date]
; Defines the default timezone used by the date functions
; http://php.net/date.timezone
date.timezone ='Asia/Shanghai'

2：在程序代码中写入

第一行写入：date_default_timezone_set ('Asia/Shanghai');
