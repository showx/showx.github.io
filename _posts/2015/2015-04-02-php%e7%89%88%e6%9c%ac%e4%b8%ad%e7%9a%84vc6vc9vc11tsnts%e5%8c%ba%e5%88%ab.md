---
layout: post
title: PHP版本中的VC6,VC9,VC11,TS,NTS区别
date: 2015-04-02 17:31
author: admin
comments: true
categories: []
---
以windows为例，看看下载到得php zip的文件名

php-5.4.4-nts-Win32-VC9-x86.zip
<div>

VC6：legacy Visual Studio 6 compiler，是使用这个编译器编译的。

VC9：Visual Studio 2008 compiler，就是这个编译器编译的。

</div>
<div>

这个其实没有什么太大的影响，因为从php 5.3，已经没有vc6版本提供下载了

TS：Thread Safe 线程安全， 执行时会进行线程（Thread）安全检查

NTS：Non Thread Safe 非线程安全， 在执行时不进行线程（Thread）安全检查

</div>
我使用Apache+PHP的模式下，一般是把PHP作为一个Module load到apache中，那么以apache父进程-多子进程的工作模式，是需要进行线程安全检查的，所以如果是以这种方式执行php，选择ts版本

那么如果是使用fastcgi，比如说用php-fpm管理php执行，则不需要进行线程安全检查，则选择nts版本的php

---------------------------------------------------------------------------------------------------------------------------php 5.5.0 beta 1发布后, 安装出现问题, 家中电脑升级是成功的, 可公司的电脑一直提示无法加载到服务. 操作系统都一样的, 没什么区别. www.php.net官网左侧的说明提醒了我.

php-5.5.0beta1-Win32-VC11-x86

安装包的名字也已经说明了, 要运行必须安装vc11, x86表示32位, 假如是x64就是64位, 位数对于安装vc11有帮助, 个人建议vc11 x86, x64两个版本都安装上比较好, 反正没冲突.

然后启动服务, 搞定. phpinfo信息如下:

-----------------------------------------------------------------------------------------------------------------------------
<div>

VC6：legacy Visual Studio 6 compiler，就是使用这个编译器编译的。

VC9：Visual Studio 2008 compiler，就是用微软的VS编辑器编译的。

由于apache.org只提供VC6的版本，所以使用原版apache时只能使用VC6。（www.apachelounge.com上有apache VC9的版本提供，应该可以和PHP VC9配合，没用过）

TS：Thread Safe 线程安全， 执行时会进行线程（Thread）安全检查，以防止有新要求就启动新线程的CGI执行方式而耗尽系统资源

NTS：Non Thread Safe 非线程安全， 在执行时不进行线程（Thread）安全检查

PHP的两种执行方式：ISAPI和FastCGI。

ISAPI(Internet Server Application Programming Interface)执行方式是以DLL动态库的形式使用，可以在被用户请求后执行，在处理完一个用户请求后不会马上消失，所以需要进行线程安全检查，这样来提高程序的执行效率，所以如果是以ISAPI来执行PHP，建议选择Thread Safe版本

apache中的配置方式：

</div>
<div>

#下面这个是加载TS版本的php必须的

LoadModule php5_module “xxx/php5apache2_2.dll”

#下面这行可有可无

</div>
AddType application/x-httpd-php-source .phpsAddType application/x-httpd-php .php .php5 .php4 .php3 .phtml .phpt
<div>

FastCGI执行方式是以单一线程来执行操作，所以不需要进行线程的安全检查，除去线程安全检查的防护反而可以提高执行效率，所以，如果是以FastCGI来执行PHP，建议选择Non Thread Safe版本。

apache中的配置方式：

</div>
<div>

#下面这两行是加载NTS版本的php必须的，不可以直接写成Action application/x-httpd-php “c:/wamp/bin/php/php3.5.6/php-cgi.exe”！

ScriptAlias /php/ "C:/wamp/bin/php/php3.5.6/"

Action application/x-httpd-php “/php/php-cgi.exe”

</div>
#另外，还要有之前的AddType application/x-httpd-php .php .php5 .php4 .php3 .phtml .phpt，这样才能认识php格式的文件

#这样配置完可能还会因为权限问题而无法用php-cgi.exe解析php网页，所以还要加上下面这段
<div>

&lt;Directory "C:/wamp/bin/php/php5.3.6/"&gt;

AllowOverride None

Options None

Order allow,deny

Allow from all

&lt;/Directory&gt;

官方并不建议你将Non Thread Safe 应用于生产环境，所以我们选择Thread Safe 版本的PHP来使用。

XAMPP在http-xampp.conf中默认配置是使用ISAPI的方式

</div>
