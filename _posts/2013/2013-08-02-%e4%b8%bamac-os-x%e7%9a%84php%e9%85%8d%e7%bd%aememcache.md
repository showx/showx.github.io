---
layout: post
title: 为Mac OS X的PHP配置Memcache
date: 2013-08-02 17:39
author: admin
comments: true
categories: []
---
php的memcache扩展安装包下载地址：http://pecl.php.net/package/memcache

首先安装memcache主程序，解压之后，terminal->cd切到memcached目录，install-sh，install安装完成，在环境变量里添加EVENT_NOKQUEUE=1，验证是否成功， $memcached -h，应该产生帮助提示

启动memcached命令：memcached -m 32 -p 11211-d

然后安装memcache，解压之后：

phpize

./configure

make

sudo make install

之后会在/usr/lib/php/extensions/no-debug-non-zts-20090626里生成memcache.so文件，接下来修改php.ini，加入

extension=/usr/lib/php/extensions/no-debug-non-zts-20090626/memcache.so

保存并重启apache，访问info.php可以看到php已经支持memcache
