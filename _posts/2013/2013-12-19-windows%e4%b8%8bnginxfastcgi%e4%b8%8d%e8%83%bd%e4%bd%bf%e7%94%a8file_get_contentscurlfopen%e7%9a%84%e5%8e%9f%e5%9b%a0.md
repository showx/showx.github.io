---
layout: post
title: windows下nginx+fastcgi不能使用file_get_contents/curl/fopen的原因  
date: 2013-12-19 15:27
author: admin
comments: true
categories: []
---
这两天一直在搞windows下nginx+fastcgi的file_get_contents请求。我想，很多同学都遇到当file_get_contents请求外网的http/https的php文件时毫无压力，比如echo file_get_contents(‘http://www.baidu.com’)，它会显示百度的页面。但当你请求localhost/127.0.0.1本地网络的php服务时却一直是timeout，无论你将请求时间和脚本运行时间多长都无法返回数据，如file_get_contents(‘http://localhost/phpinfo.php’)。然而当你尝试请求html这样的静态文件时却完全没有问题。是什么原因呢？！

首先，我们知道file_get_contents/curl/fopen打开一个基于tcp/ip的http请求时，请求数据发送到nginx，而nginx则委托给php-cgi(fastcgi)处理php文件，一般情况fastcgi处理完一个php请求后会马上释放结束信号，等待下一个处理请求（当然也有程序假死，一直占用资源的情况）。打开nginx.conf，我们看到下面这一行：


location ~ \.php {
        fastcgi_pass   127.0.0.1:9000;
        fastcgi_index  index.php;
        fastcgi_param  SCRIPT_FILENAME  d:/www/htdocs$fastcgi_script_name;
        include        fastcgi_params;
}

上面已经清楚地看到，所有使用php结尾的文件都经过fastcgi处理，而在php.ini的配置文件中也有一句：

cgi.force_redirect = 1

表明，所有php程序安全地强制转向交给cgi处理。

但在windows中，本地127.0.0.1:9000怎样与php-cgi联系的呢？！答案是增加一个php-cgi进程，用它来监听127.0.0.1:9000。通过控制器命令：

RunHiddenConsole.exe D:/www/php/php-cgi.exe  -b 127.0.0.1:9000 -c C:/WINDOWS/php.ini

我们就可以在启动windows时，开启一个php-cgi.exe进程监听来自127.0.0.1:9000 的请求。在dos命令下打开netstat –a就可以看到本地计算机下的9000端口处于listening状态（也就是空置，如果没有发送任何请求的话）。

好了，该说说在php中使用file_get_contents()、curl()、fopen()函数访问localhost时为什么不能返回结果。我们再来试验在index.php中加入file_get_contents(‘http://127.0.0.1/phpinfo.php’)语句向phpinfo.php发送一个请求，这时浏览器中的状态指示一直在打转，表示它一直在工作中。打开Dos中的netstat命令，可以看到本地的9000端口的状态为：ESTABLISHED，表示该进程在联机处理中。实际上，这里我们已经同时向nginx发送了两个基于http的php请求，一个是解析index.php，而另一个是phpinfo.php，这样矛盾就出来了，因为我们的windows系统只加载了一个php-cgi进程，因此，它无法同时处理php请求，它只能先处理第一个请求（index.php），而index.php却又在等待phpinfo.php处理结果，phpinfo.php没人帮它处理请求，因为它一直在等待index.php释放结束信号，因此，造成了程序的阻塞状态，陷入了死循环。所以我们就看到了浏览器的状态指示一直在打转。Curl()与fopen函数的原因也相同。

找到了原因，我们也就有了解决办法。

一是，向系统增加一个php-cgi进程，当一个php-cig繁忙时，它能够处理其它php请求。这时需给另一个fastcgi进程分配同一ip的不同端口，比如9001。为了解决两个进程的工作分配情况，在nginx中可以通过upstream模块处理它们的工作权重，这也是很多网站实现负载均衡的解决方案：


upstream bakend {

server 172.0.0.1:9000 weight = 5 max_fails=0 fail_timeout=30s;

server 172.0.0.1:9001 weight = 5 max_fails=0 fail_timeout=30s;

}

location ~ \.php {

fastcgi_pass bakend;

fastcgi_index index.php;

fastcgi_param SCRIPT_FILENAME d:/www/htdocs$fastcgi_script_name;

include fastcgi_params;

}

这样，端口9000与9001均分到了均等的处理机会，当一个进程（sever）繁忙时，由nginx调配另一个进程处理php程序(这里nginx还有其它原理与实现)。

另一个问题是，手动让系统管理多个php-cgi程序比较麻烦，特别是在远程管理的时候尤为不方便，这时可以通过php-fpm或spawn-fcgi来代替，但在windows中并没有php-fpm的二进制编译版本，只能在unix/linux中使用。具体查看各自的手册。

二是，看请求的程序是否一定需要动态解析，如果不是可以转为html或js的格式。如果file_get_contents(‘http://127.0.0.1/a.html’)则不需要通过fastcgi来处理，也就不会出现阻塞的情况。

三是，php的程序取消交由fastcgi，这样则使每个php请求都会自动打开一个php进程，但不好的方面是php-cgi的优势没有了。

 

另外提醒下，网上有人说，通过去掉地址中的http://协议标记，而使用相对地址就规避函数的检查，实际情况是不是这样呢？！当在index.php中使用file_get_contents(‘phpinfo.php’);时，我们可以看到函数输出了phpinfo.php的源代码，相当于file_get_contents(‘file:\\\c:\www\phpinfo.php’);，它实际上只是读取你的文本内容，因为file_get_contents()函数首先是处理file协议的，而curl则直接报错无法解析。因此这些人纯粹是不学无术的骗子。

还有人提出修改hosts文件，增加localhost www.xxx.com影射关系，函数通过www.xxx.com访问本地php，这其实也是不治本的偏方，因为这只是方便计算机的dns解析，最终www.xxx.com交给127.0.0.1，而后者交给fastcgi，还是阻塞。
