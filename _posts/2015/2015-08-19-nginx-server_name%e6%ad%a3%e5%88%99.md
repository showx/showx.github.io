---
layout: post
title: nginx server_name正则
date: 2015-08-19 13:21
author: admin
comments: true
categories: []
---
Nginx中的server_name指令主要用于配置基于名称的虚拟主机，server_name指令在接到请求后的匹配顺序分别为：

1、准确的server_name匹配，例如：

&nbsp;
<pre>server {
     listen       80;
     server_name  domain.com  www.domain.com;
     ...
}</pre>
&nbsp;

&nbsp;

2、以*通配符开始的字符串：
<pre>server {
     listen       80;
     server_name  *.domain.com;
     ...
}
</pre>
3、以*通配符结束的字符串：
<pre>server {
     listen       80;
     server_name  www.*;
     ...
}
</pre>
4、匹配正则表达式：
<pre>server {
     listen       80;
     server_name  ~^(?.+)\.domain\.com$;
     ...
}</pre>
<div>nginx将按照1,2,3,4的顺序对server name进行匹配，只有有一项匹配以后就会停止搜索，所以我们在使用这个指令的时候一定要分清楚它的匹配顺序（类似于location指令）。</div>
<div>server_name指令一项很实用的功能便是可以在使用正则表达式的捕获功能，这样可以尽量精简配置文件，毕竟太长的配置文件日常维护也很不方便。下面是2个具体的应用：</div>
<div>1、在一个server块中配置多个站点：</div>
<pre>server
   {
     listen       80;
     server_name  ~^(www\.)?(.+)$;
     index index.php index.html;
     root  /data/wwwsite/$2;
   }
</pre>
站点的主目录应该类似于这样的结构：
<pre>/data/wwwsite/domain.com
/data/wwwsite/nginx.org
/data/wwwsite/baidu.com
/data/wwwsite/google.com</pre>
&nbsp;

这样就可以只使用一个server块来完成多个站点的配置。

2、在一个server块中为一个站点配置多个二级域名。

实际网站目录结构中我们通常会为站点的二级域名独立创建一个目录，同样我们可以使用正则的捕获来实现在一个server块中配置多个二级域名：

&nbsp;
<pre>server
   {
     listen       80;
     server_name  ~^(.+)?\.domain\.com$;
     index index.html;
     if ($host = domain.com){
         rewrite ^ http://www.domain.com permanent;
     }
     root  /data/wwwsite/domain.com/$1/;
   }
</pre>
站点的目录结构应该如下：
<pre>/data/wwwsite/domain.com/www/
/data/wwwsite/domain
.com/nginx/</pre>
这样访问www.domain.com时root目录为/data/wwwsite/domain.com/www/，nginx.domain.com时为/data/wwwsite/domain.com/nginx/，以此类推。

后面if语句的作用是将domain.com的方位重定向到www.domain.com，这样既解决了网站的主目录访问，又可以增加seo中对www.domain.com的域名权重。

dns指派也可以，例如： dx*.sh-ow.com  dx2.sh-ow.com dx3.sh-ow.com等通用。
