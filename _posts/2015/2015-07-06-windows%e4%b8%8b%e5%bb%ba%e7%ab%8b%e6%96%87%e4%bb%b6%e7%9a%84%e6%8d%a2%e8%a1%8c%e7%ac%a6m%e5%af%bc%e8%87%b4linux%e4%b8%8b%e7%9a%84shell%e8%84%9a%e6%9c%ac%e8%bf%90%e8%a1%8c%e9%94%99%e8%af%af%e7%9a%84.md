---
layout: post
title: windows下建立文件的换行符^M导致linux下的shell脚本运行错误的解决方案
date: 2015-07-06 11:13
author: admin
comments: true
categories: []
---
经常在windows下编辑的文件远程传送到linux下的时候每行末尾都会出现^M，这将导致shell脚本运行错误，主要是因为<strong>dos下的编辑器和linux下的编辑器对文件末行的回车符处理不一致导致</strong>。

主要解决如下：

（1）<strong>在VI编辑器中将^M删除</strong>：

将VI编辑器切换到命令模式下，输入 :%s/^M//g (注意^M 不是shift ^ +M 而是ctrl+v 加上ctrl+m)  <strong>s///g</strong>是shell的<strong>替换</strong>命令

此命令必须是手动打上，不可复制。

（2）<strong>dos2unix 命令</strong>

dos2unix filename

（2）使用支持修改换行符的编辑器另存为一下文件,比如emeditor
<img src="http://img.phperz.com/data/img/20141013/14131927807518.jpg" alt="" />
