---
layout: post
title: ubuntu php7安装memcache注意事项
date: 2017-02-27 16:03
author: admin
comments: true
categories: [Uncategorized]
---
1.要安装libmemcache-dev libmemcached-dev库
2.error: no, sasl.h is not available. Run configure with –disable-memcached-sasl to disable this check
- 在./configure加上--disable-memcached-sasl即可
- 3.git clone https://github.com/websupport-sk/pecl-memcache memcache
cd memcache
phpize
make &amp;&amp; make install
在php.ini上启用memcache.so完成
对于libmemcache搜索不了,善用apt-cache search memcache就会列出所有
