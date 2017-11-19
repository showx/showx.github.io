---
layout: post
title: 基于libmemcached，php扩展memcached的安装
date: 2015-01-23 17:18
author: admin
comments: true
categories: []
---
<strong>一，为什么要装memcached扩展</strong>

memcached的1.2.4及以上增加了CAS(Check and Set)协议,对于同一key的多进行程的并发处理问题。这种情况其实根数据库很像，如果同时有几个进程对同一个表的同一数据进行更新的话，那会不会打架呢，哈哈。数据库里面可以锁定整张表，也可以锁定表里面一 行的功能，其实memcached加入的CAS根这个差不多。

php的扩展memcache，不支持cas，所以我们要装memcached扩展，memcached扩展是基于libmemcached，所以要先安装libmemcached

<strong>二，查看memcahced的版本信息</strong>

telnet 127.0.0.1 12000
stats
你会看到有以下信息
STAT pid 15322
STAT uptime 1885
STAT time 1279455772
STAT version 1.2.8
STAT pointer_size 32
如果版本过低，考虑重新装一下

退出telnet ，<strong>ctrl + ] 然后在按q就行了。</strong>

<strong>三，安装所要软件</strong>

wget <a title="libmemcached" href="http://launchpad.net/libmemcached/1.0/0.42/+download/libmemcached-0.42.tar.gz" target="_blank">http://launchpad.net/libmemcached/1.0/0.42/+download/libmemcached-0.42.tar.gz</a>

wget <a title="php memcached" href="http://pecl.php.net/get/memcached-1.0.2.tgz" target="_blank">http://pecl.php.net/get/memcached-1.0.2.tgz</a>

memcached的官方网站 <a title="memcached官方网站" href="http://www.memcached.org/" target="_blank">http://www.memcached.org/</a>

<strong>四，安装libmemcached</strong>

tar zxvf libmemcached-0.42.tar.gz
cd libmemcached-0.42
./configure --prefix=/usr/local/libmemcached  --with-memcached
make &amp;&amp; make install

安装要注意的问题：

1，  安装过程中不要忘了，--with-memcached，不然会提示你

checking for memcached... no
configure: error: "could not find memcached binary"

2，你的memcached是不是1.2.4以上的，如果不是会提示你

clients/ms_thread.o: In function `ms_setup_thread':
/home/zhangy/libmemcached-0.42/clients/ms_thread.c:225: undefined reference to `__sync_fetch_and_add_4'
clients/ms_thread.o:/home/zhangy/libmemcached-0.42/clients/ms_thread.c:196: more undefined references to `__sync_fetch_and_add_4' follow
collect2: ld returned 1 exit status
make[2]: *** [clients/memslap] Error 1
make[2]: Leaving directory `/home/zhangy/libmemcached-0.42'

解决办法是--disable-64bit CFLAGS="-O3 -march=i686"，如果不用这个64位的long型数据，我想php扩展memcached，memcache也就没什么区别了，装memcached也就没什么意思了。

<strong>五，php的扩展memcached的安装</strong>

tar zxvf memcached-1.0.2.tar.gz
cd memcached-1.0.2
/usr/local/php/bin/phpize
./configure --enable-memcached --with-php-config=/usr/local/php/bin/php-config --with-libmemcached-dir=/usr/local/libmemcached
./configure --prefix=/usr/local/phpmemcached  --with-memcached
make &amp;&amp; make install

vi /usr/local/php/lib/php.ini

加上extension=memcached.so重起一下服务就可以了。
