---
layout: post
title: Nginx服务器unknown directive echo的解决办法
date: 2015-04-30 13:52
author: admin
comments: true
categories: []
---
<table width="100%">
<tbody>
<tr>
<td>
<div id="contentMidPicAD"></div>
实际上，Nginx并没有echo这个指令，所以你贸然使用时，自然会提示说无法识别的指令，处理方法有两个：

方法一是：
从下面连接下载echo-nginx-module模块并安装：
https://github.com/agentzh/echo-nginx-module

下载之后安装按照普通模块安装即可：
./configure --prefix=/usr/local/nginx --add-module=/dir-to-echo-nginx-module
安装之后，root权限执行：
make -j2 &amp;&amp; make install
然后就可以在自定义模块中使用echo指令。
比如在配置文件nginx.conf中添加：
location /hello {
echo "hello, use echo!";
}

方法2是：
不用使用echo指令，直接使用html之类的文件代替，但是url需要添加（.html）后缀名，例如：
location /hello.html {
root     html;
index    htllo.html;
}</td>
</tr>
</tbody>
</table>
