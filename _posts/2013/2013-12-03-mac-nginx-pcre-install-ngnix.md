---
layout: post
title:  Mac nginx PCRE install ngnix
date: 2013-12-03 21:38
author: admin
comments: true
categories: []
---
装nginx的时候报错：
./configure: error: the HTTP rewrite module requires the PCRE library.
虽然可以跳过rewrite功能，但是这个功能还是很适用的，网上找了很多方法，都是yum install pcre-devel 
发现也没有yum这个命令，在linux里倒有。后来找了半天才找到以下方法：现在想想其实挺简单的，哎~
1、访问ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre

下载完成之后，使用CD命令进入到相应的下载文件夹，执行命令：

sudo tar xvfz pcre-8.12.tar.gz  解压文件
解压完成之后，执行命令

cd pcre-8.12
sudo ./configure --prefix=/usr/local --enable-utf8 
sudo make 
sudo make install 
然后可以回到ngnix文件安装ngnix


2、下载最新的pcre-8.12.tar.gz或者直接在bash里
程序代码

sudo mkdir -p /usr/local/src  
cd /usr/local/src  
sudo curl -O ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.12.tar.gz  
sudo tar xvfz pcre-8.12.tar.gz  
cd pcre-8.12
sudo ./configure --prefix=/usr/local --enable-utf8 
sudo make 
sudo make install 
 


这样就可以再次安装nginx了

 

安装Nginx
准备工作做好了，这一步就很简单了，首先将nginx-1.2.0.tar.gz放在/usr/local/src目录下，执行下面的命令即可。相应的一些参数可以按照个人需求修改。

$ tar xvzf nginx-1.2.0.tar.gz
$ cd nginx-1.2.0
$ sudo ./configure --prefix=/usr/local/nginx --with-http_ssl_module
$ sudo make
$ sudo make install
PCRE 介绍：

The PCRE library is a set of functions that implement regular expression pattern matching using the same syntax and semantics as Perl 5. PCRE has its own native API,
