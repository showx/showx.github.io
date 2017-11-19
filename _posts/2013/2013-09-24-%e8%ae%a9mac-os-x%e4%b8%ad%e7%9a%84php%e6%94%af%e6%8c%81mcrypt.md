---
layout: post
title: 让Mac OS X中的PHP支持mcrypt
date: 2013-09-24 20:51
author: admin
comments: true
categories: []
---
<a href="http://mcrypt.sourceforge.net/">MCrypt</a>是一个功能强大的加密算法扩展库，它包括有22种算法，phpMyAdmin依赖这个PHP扩展，具体如下：
<ul>
	<li>下载并解压<a href="http://sourceforge.net/projects/mcrypt/files/Libmcrypt/">libmcrypt-2.5.8.tar.gz</a>。</li>
	<li>在终端执行如下命令：
<div>tar zxvf libmcrypt-2.5.8.tar.gz
cd libmcrypt-2.5.8/
./configure --disable-posix-threads --enable-static
make
sudo make install</div></li>
	<li>下载并解压<a href="http://cn.php.net/get/php-5.3.4.tar.gz/from/a/mirror">PHP源码文件php-5.3.4.tar.gz</a>。Mac OS X 10.6.3中预装的PHP版本是5.3.4，所以需要下载这个版本。http://us2.php.net/get/php-5.3.13.tar.bz2/from/a/mirror</li>
	<li>在终端执行如下命令：
<div>tar zxvf php-5.3.4.tar.gz
cd php-5.3.4/ext/mcrypt
phpize
./configure
make
sudo cp modules/mcrypt.so /usr/lib/php/extensions/no-debug-non-zts-20090626/</div></li>
</ul>
&nbsp;
<ol>
	<li>在终端执行如下命令，把php-5.3.13.tar.bz2，并<a href="http://www.coolestguyplanettech.com/how-to-install-mcrypt-for-php-on-mac-osx-lion-10-7-development-server/">配置autoconf</a>（在新的Mac OS X的Xcode中需要自己配置），然后才能运行<code>phpize</code>命令：
<pre><code>cd ~/Downloads
tar -zxvf php-5.3.13.tar.bz2

cd php-5.3.13/ext/mcrypt
curl -O http://ftp.gnu.org/gnu/autoconf/autoconf-latest.tar.gz
tar -zxvf autoconf-latest.tar.gz
cd autoconf-2.69
./configure
make
sudo make install

cd ..
phpize
./configure
make
sudo make install
</code></pre>
</li>
	<li>打开php.ini
<pre><code>sudo vi /etc/php.ini</code></pre>
在php.ini中加入如下代码，并保存后退出，然后重启Apache
<pre><code>extension=mcrypt.so</code></pre>
</li>
</ol>
<ul>
	<li>打开php.ini
<div>sudo vi /etc/php.ini</div>
在php.ini中加入如下代码，并保存后退出，然后重启Apache
<div>extension=/usr/lib/php/extensions/no-debug-non-zts-20090626/mcrypt.so</div></li>
</ul>
