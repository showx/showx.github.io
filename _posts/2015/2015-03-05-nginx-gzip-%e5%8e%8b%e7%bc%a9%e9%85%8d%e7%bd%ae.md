---
layout: post
title: Nginx Gzip 压缩配置
date: 2015-03-05 16:09
author: admin
comments: true
categories: []
---
随着nginx的发展，越来越多的网站使用nginx，因此nginx的优化变得越来越重要，今天我们来看看nginx的gzip压缩到底是怎么压缩的呢？

gzip(GNU-ZIP)是一种压缩技术。经过gzip压缩后页面大小可以变为原来的30%甚至更小，这样，用户浏览页面的时候速度会块得多。gzip的压缩页面需要浏览器和服务器双方都支持，实际上就是服务器端压缩，传到浏览器后浏览器解压并解析。浏览器那里不需要我们担心，因为目前的巨大多数浏览器都支持解析gzip过的页面。
Nginx的压缩输出有一组gzip压缩指令来实现。相关指令位于http{….}两个大括号之间。

gzip on;
//该指令用于开启或关闭gzip模块(on/off)

gzip_min_length 1k;
//设置允许压缩的页面最小字节数，页面字节数从header头得content-length中进行获取。默认值是0，不管页面多大都压缩。建议设置成大于1k的字节数，小于1k可能会越压越大。

gzip_buffers 4 16k;
//设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。4 16k代表以16k为单位，安装原始数据大小以16k为单位的4倍申请内存。

gzip_http_version 1.1;
//识别http的协议版本(1.0/1.1)

gzip_comp_level 2;
//gzip压缩比，1压缩比最小处理速度最快，9压缩比最大但处理速度最慢(传输快但比较消耗cpu)

gzip_types text/plain application/x-javascript text/css application/xml
//匹配mime类型进行压缩，无论是否指定,”text/html”类型总是会被压缩的。
gzip_vary on;
//和http头有关系，加个vary头，给代理服务器用的，有的浏览器支持压缩，有的不支持，所以避免浪费不支持的也压缩，所以根据客户端的HTTP头来判断，是否需要压缩

nginx 配置gzip段如下：
gzip on;
gzip_min_length 1k;
gzip_buffers 16 64k;
gzip_http_version 1.1;
gzip_comp_level 6;
gzip_types text/plain application/x-javascript text/css application/xml;
gzip_vary on;
