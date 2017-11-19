---
layout: post
title: linux下shell脚本执行php报Could not open input file
date: 2015-08-03 11:02
author: admin
comments: true
categories: []
---
  今天在linux下通过svn更新了一个sh文件,是想通过shell脚本来通过执行php插入一个数据到数据库,但没有想到居然报Could not open input file这个错误,开始我以为是没有给sh脚本权限问题,使用chmod进行更改后,还是报错.如下图
点击查看原图
后来网上搜了下才知道是文件格式问题,出错文件的格式是dos,可以在vi中使用:set ff来查看格式,如图
点击查看原图
如果是dos格式,那么就要使用:set ff=unix来设置新格式,如图
点击查看原图
再使用:set ff来查看格式,可以看到已经是unix的格式了,如图
点击查看原图
再来执行sh脚本,可以看到脚本成功执行了.
点击查看原图
