---
layout: post
title: 解决libpython2.6.so.1.0: cannot open shared object file
date: 2015-06-19 10:21
author: admin
comments: true
categories: []
---
文章解决的问题：安装nginx中需要Python2.6的支持，下面介绍如何安装Python2.6，并建立lib的连接。

问题展示：error while loading shared libraries: libpython2.6.so.1.0: cannot open shared object file: No such file or directory

解决方案：

1. 安装Python2.6

1.1 下载Python-2.6.6.tgz，下载地址：http://www.python.org/ftp/python/2.6.6/Python-2.6.6.tgz

1.2 解压tgz ：
[plain] view plaincopy

    tar -xzvf Python-2.6.6.tgz  


1.3 cd 到解压后的文件夹中，进行Python的make 安装
[plain] view plaincopy

    ./configure --prefix=/foo/python26 --enable-shared  
    make  
    make altinstall  

2. 建立lib连接：sudo ln -s /foo/python26/lib/libpython2.6.so.1.0  /usr/lib/libpython2.6.so.1.0


============

安装了python2.7，第一次执行时报错：
error while loading shared libraries: libpython2.7.so.1.0: cannot open shared object file: No such file or directory

解决方法如下：
1.编辑      vi /etc/ld.so.conf 
如果是非root权限帐号登录，使用 sudo vi /etc/ld.so.conf 
添加上python2.7的lib库地址，如我的/usr/local/Python2.7/lib，保存文件

2.执行 /sbin/ldconfig -v命令，如果是非root权限帐号登录，使用 sudo  /sbin/ldconfig -v。这样 ldd 才能找到这个库，执行python2.7就不会报错了

/etc/ld.so.conf:
这个文件记录了编译时使用的动态链接库的路径。
默认情况下，编译器只会使用/lib和/usr/lib这两个目录下的库文件
如果你安装了某些库，没有指定 --prefix=/usr 这样lib库就装到了/usr/local下，而又没有在/etc/ld.so.conf中添加/usr/local/lib，就会报错了

ldconfig是个什么东东吧 ：
它是一个程序，通常它位于/sbin下，是root用户使用的东东。具体作用及用法可以man ldconfig查到
简单的说，它的作用就是将/etc/ld.so.conf列出的路径下的库文件 缓存到/etc/ld.so.cache 以供使用
因此当安装完一些库文件，(例如刚安装好glib)，或者修改ld.so.conf增加新的库路径后，需要运行一下/sbin/ldconfig
使所有的库文件都被缓存到ld.so.cache中，如果没做，即使库文件明明就在/usr/lib下的，也是不会被使用的，结果
编译过程中抱错，缺少xxx库。

