---
layout: post
title: Windows cmd下建立.htaccess文件
date: 2014-07-16 14:22
author: admin
comments: true
categories: []
---
Windows 图形下是不能直接建立空名字的内容的, 如.htaccess 在windows explorer 看来就只有后缀名没有文件名.

不过可以用windows 命令行来建立文件 打开cmd终端

[plain] view plaincopy
cd /d 需要建立文件的文件夹  
type nul>.htaccess  

这样.htaccess 文件就建立好了,随便找个文本编辑器添加内容吧
