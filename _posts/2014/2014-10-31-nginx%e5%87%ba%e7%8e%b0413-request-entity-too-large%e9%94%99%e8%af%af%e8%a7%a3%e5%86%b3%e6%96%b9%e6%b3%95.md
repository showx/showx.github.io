---
layout: post
title: Nginx出现413 Request Entity Too Large错误解决方法
date: 2014-10-31 11:00
author: admin
comments: true
categories: []
---
Nginx出现的413 Request Entity Too Large错误,这个错误一般在上传文件的时候出现，打开nginx主配置文件nginx.conf，找到http{}段，添加
解决方法就是
打开nginx主配置文件nginx.conf，一般在/usr/local/nginx/conf/nginx.conf这个位置，找到http{}段，修改或者添加
 代码如下	复制代码
client_max_body_size 2m;
然后重启nginx，
 代码如下	复制代码

sudo /etc/init.d/nginxd reload
 即可。
要是以php运行的话，这个大小client_max_body_size要和php.ini中的如下值的最大值差不多或者稍大，这样就不会因为提交数据大小不一致出现错误。
 代码如下	复制代码
post_max_size = 2M
upload_max_filesize = 2M
重启NGINX
 代码如下	复制代码
kill -HUP `cat /usr/local/nginx/nginx.pid `
恢复正常
