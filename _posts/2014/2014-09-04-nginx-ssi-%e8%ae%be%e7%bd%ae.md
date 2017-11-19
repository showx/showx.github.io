---
layout: post
title: Nginx ssi 设置
date: 2014-09-04 16:46
author: admin
comments: true
categories: []
---
一：为什么用ssi?

一个登录用户在页面访问的时候如何充分利用 cache?

页面静态化的一个大问题是登录用户访问页面如何静态化。例如首页，大部分的页面内容需要缓存但是用户登录后的个人信息是动态信息，不能缓存。那么如何解决这个"页面部分缓存"问题？

现有的方案是利用 SSI - Server Side include.

Nginx SSI 实现是 http://wiki.nginx.org/NginxHttpSsiModule

这里最关键的就是静态文件可以包含一个动态的网页的 URL.

二：配置ssi

主要是三个参数，ssi，ssi_silent_errors和ssi_types，均可以放在http,server和location的作用域下。

ssi on
开启ssi支持，默认是off

ssi_silent_errors on
默认值是off，开启后在处理SSI文件出错时不输出错误提示:"[an error occurred while processing the directive] "

ssi_types 
默认是ssi_types text/html，所以如果需要htm和html支持，则不需要设置这句，如果需要shtml支持，则需要设置：ssi_types text/shtml

SSI的格式：
< !--#include file="bottom.htm"-->
或
< !--#include virtual="/hx/bottom.htm"-->
路径是相对server中root根目录。

更多请参见官方文档：http://wiki.nginx.org/NginxChsHttpSsiModule

示例：

1.开启shtml后缀的文件名支持ssi
server{
    ......
    ssi on;
    ssi_silent_errors on;
    ssi_types text/shtml;
}

2.开启html后缀的文件名支持ssi
server{
    ......
    ssi on;
    ssi_silent_errors on;
}

3.在zt目录下开启html后缀的文件名支持ssi
server{
    ......
    location /hx/{
        ssi on;
        ssi_silent_errors on;
    }
}
