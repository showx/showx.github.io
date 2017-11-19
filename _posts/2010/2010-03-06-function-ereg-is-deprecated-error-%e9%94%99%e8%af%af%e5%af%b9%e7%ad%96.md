---
layout: post
title: Function ereg() is deprecated Error 错误对策
date: 2010-03-06 04:51
author: admin
comments: true
categories: []
---
错误： Deprecated: Function ereg() is deprecated in …… 解决方法一：退回去用php5.2。（众人皆赞道：果是好法子！） 解决方法二：继续用php5.3，但是修改devel/devel.modul的460行： if ($errno &amp; (E_ALL ^ E_NOTICE)) { 改为 if ($errno &amp; (E_ALL &amp; ~E_NOTICE &amp; ~E_DEPRECATED)) { 把丫deprecated错误给忽略掉。（众人皆又赞道：果……果……果是好法子！） 解决方法三：动程序鸟，把ereg换成preg_match，ereg_replace也需得换成preg_replace。只得注意的是 ereg(’^[0-9]‘ 需修改成 preg_match(’/^[0-9]/‘
