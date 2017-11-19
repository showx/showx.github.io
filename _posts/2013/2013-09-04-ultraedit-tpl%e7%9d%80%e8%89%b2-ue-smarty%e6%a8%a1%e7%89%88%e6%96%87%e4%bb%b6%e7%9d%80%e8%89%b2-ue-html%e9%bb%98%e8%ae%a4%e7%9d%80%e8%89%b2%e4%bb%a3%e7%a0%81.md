---
layout: post
title: ultraedit tpl着色 ue smarty模版文件着色 ue html默认着色代码
date: 2013-09-04 09:58
author: admin
comments: true
categories: []
---
        这几天用smarty模版写点东西，没再用eclipse编辑器，直接用ultraedit编辑，发现对smarty默认.tpl文件不着色，我想改成.html默认着色怎么办呢？（ue的版本是15.20）
方法如下：
   在高级-》配置-》编辑器显示-》语法着色，找到文档的完整路径名
如下：C:\Documents and Settings\你的用户名\Application Data\IDMComp\UltraEdit\wordfiles
打开这个文件夹下的文件：html.uew,在第一行的最后加上 tpl就OK.
如下 ： File Extensions = HTM HTML SHTML HTT HTA HTX CFM JSP PHP PHTML ASP TMPL TPL
mac文件夹在:/Users/show/Library/Application Support/UltraEdit/
