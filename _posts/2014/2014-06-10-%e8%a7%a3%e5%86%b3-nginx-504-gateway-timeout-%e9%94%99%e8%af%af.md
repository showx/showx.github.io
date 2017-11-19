---
layout: post
title: 解决 Nginx 504 Gateway Timeout 错误
date: 2014-06-10 23:16
author: admin
comments: true
categories: []
---
近期我的博客有时会出现 504 Gateway Timeout 错误，查看了下 Nginx 的文档，有两个参数引起了我的关注。

一个是 send_timeout，该参数指定客户端的响应超时时间，它不适用于整个传输过程，只适用于两个客户端读操作之间的过程。当客户端在该时间内没有读取任何数据，Nginx 将关闭连接；另一个是 fastcgi_read_timeout，该参数指定上游端等待 fastcgi 进程发送数据的最大时间。

可以看出，出现 Nginx 504 Gateway Timeout 错误是因为 fastcgi_read_timeout 设置的过小造成的，在设定的时间内，fastcgi 进程结束后并未产生任何输出。

解决方法很简单，适当增大 fastcgi_read_timeout，默认为60s，可根据站点的实际情况调至120s甚至更大。

fastcgi_read_timeout 120s;
