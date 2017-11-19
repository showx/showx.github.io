---
layout: post
title: docker vagrant页面错误解决
date: 2015-08-17 21:27
author: admin
comments: true
categories: []
---
之前提到说通过Vagrant部署开发环境，使用目录共享模式，在本地磁盘进行开发，而通过虚拟机环境运行开发的页面。

是的，一切看起来都是那么的顺利，首先基于VirtualBox安装了Vagrant，接下来，按照以往部署环境的习惯，在VM中安装了nginx作为开发运行环境，并且将本地的共享目录作为nginx的web目录，然后打开页面，看上去似乎都很正常，但接下来，你发现了一个神奇的事情，你修改替换了一个css，一张图片，然后刷新浏览器，发现什么都没有变，然后你有非常猛烈、使劲的F5，依旧还是没有改变，是的，你看看编辑器，似乎替换是正常的，在看看VM上的文件，也都是对的，是的，尝试重启nginx，依旧没有任何变化，你开始怀疑php5-fpm甚至于毫不相干的memcached和mysql，但都无济于事。也不知道是什么让这些文件被“缓存”了呢。

当你尝试修改一个js，并且用同样的方法更新之后，会遇到类似的问题，是的，就算重启VM上任何服务，甚至重启VM，依旧没有用，当然，比起其他资源文件，浏览器的反应会强烈一些，因为浏览器会提示未知错误，而你通过浏览器查看你修改的JS文件，会看到文件尾巴有下面奇怪的随机字符：

�����������������

这到底是什么东西呢？编码错误？缓存异常？又或是其他什么？

是的，你尝试花费很多时间，试验各种各样的方法去解决这个问题，其实对于nginx来说，你只需要修改配置文件(nginx.conf)中的一行重启就能简单的解决这个问题：

sendfile off;
找到 nginx.conf ，把里面的 “sendfile on” 修改为 “sendfile off”。

当然，如果你使用Apache也可能遇到类似的问题，那么同样也有类似的配置需要修改：

EnableSendfile off

＝＝＝＝＝
http://stackoverflow.com/questions/9479117/vagrant-virtualbox-apache2-strange-cache-behaviour
