---
layout: post
title: NGINX: 405 Not Allowed
date: 2014-02-27 10:47
author: admin
comments: true
categories: []
---
今天碰到一个dz的批量上传文件不成功的问题。
追踪发现，是把静态文件都优化了新地址导致的，用图片服务器存放了swf文件
swf文件上传文件时，就变成向静态文件做post，nginx就会返回405错误
修正域名即可解决。

另外，发现一个好玩的：

NGINX不允许向静态文件提交POST方式的请求，否则报405错误。测试方法为，使用curl向服务器上的静态文件提交POST请求：
<div>curl -d 1=1 http://localhost/version.txt</div>
得到以下结果：
<div>&lt;html&gt;
&lt;head&gt;&lt;title&gt;405 Not Allowed&lt;/title&gt;&lt;/head&gt;
&lt;body bgcolor="white"&gt;
&lt;center&gt;&lt;h1&gt;405 Not Allowed&lt;/h1&gt;&lt;/center&gt;
&lt;hr&gt;&lt;center&gt;nginx/1.0.11&lt;/center&gt;
&lt;/body&gt;
&lt;/html&gt;</div>
网上传抄的添加以下配置的解决办法不可用：
<div>error_page 405 =200 @405;
location @405
{
root /srv/http;
}</div>
一种不完美但可用的方法为：
<div>upstream static_backend {
server localhost:80;
}

server {
listen 80;

# ...

error_page 405 =200 @405;
location @405 {
root /srv/http;
proxy_method GET;
proxy_pass http://static_backend;
}
}</div>
即转换静态文件接收的POST请求到GET方式。
