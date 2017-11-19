---
layout: post
title: 微信公众平台json格式
date: 2013-10-22 17:29
author: admin
comments: true
categories: []
---
微信公众平台 json不能把编码转换成\uxxx utf-8 ,解决这个方法，可以使用：

首先把需要用到的中文

$v['text'] = urlencode($v['text']);

$ts['title'] = $v['text'];

然后再

$json = urldecode(json_encode($ts));

这些输出来的json就不会自动转\uxxxx了

&nbsp;
