---
layout: post
title: crontab修改默认编辑器
date: 2015-07-20 17:28
author: admin
comments: true
categories: []
---
crontab修改默认编辑器
 
crontab默认编辑器为nano.
  www.anyway.cn
修改crontab默认编辑器为vi或者其他的编辑器。
 
法一：
 
export EDITOR="/usr/bin/vim" ; crontab -e
 
法二：
 
执行命令：select-editor
 
然后选择编辑器.
