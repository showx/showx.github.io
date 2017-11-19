---
layout: post
title: Nginx 开启 stub_status 模块监控
date: 2014-09-15 11:22
author: admin
comments: true
categories: []
---
Nginx中的stub_status模块主要用于查看Nginx的一些状态信息. 
本模块默认是不会编译进Nginx的,如果你要使用该模块,则要在编译安装Nginx时指定:
./configure –with-http_stub_status_module 
Java代码  
[root@10.10.90.97 ~]# ./configure --prefix=/usr/local/nginx --with-http_stub_status_module   
[root@10.10.90.97 ~]# make && make install   
查看已安装的 Nginx 是否包含 stub_status 模块
#/usr/local/nginx/sbin/nginx -V 
nginx version: nginx/0.6.32  
built by gcc 3.4.6 20060404 (Red Hat 3.4.6-10)  
configure arguments: --user=nginx --group=nginx --prefix=/home/nginx --with-http_stub_status_module   
 可以看到我安装了这个模块。注意是-V -v的话只会显示版本nginx version: nginx/0.6.32 
 开始配置nginx，在server块中加入location 就行了 
server{  
         location /nginx-status {  
             allow --------  
             allow --------//允许的ip  
             deny all;//  
             stub_status on;  
             access_log  off;  
        }  
}  
重启nginx   
killall -s HUP nginx  
然后请求www.domain.com/nginx-status 就行了，下面是结果 
Active connections: 5   
server accepts handled requests  
 5970806143 5970806143 7560482010   
Reading: 0 Writing: 5 Waiting: 0   
Active connections: 对后端发起的活动连接数.
Server accepts handled requests: Nginx总共处理了38810620个连接,成功创建38810620次握手(证明中间没有失败的),总共处理了298655730个请求.
Reading: Nginx 读取到客户端的Header信息数.
Writing: Nginx 返回给客户端的Header信息数.
Waiting: 开启keep-alive的情况下,这个值等于 active – (reading + writing),意思就是Nginx已经处理完成,正在等候下一次请求指令的驻留连接.
所以,在访问效率高,请求很快被处理完毕的情况下,Waiting数比较多是正常的.如果reading +writing数较多,则说明并发访问量非常大,正在处理过程中.
