---
layout: post
title: nginx php 配置好后网页输出空白
date: 2015-06-16 15:22
author: admin
comments: true
categories: []
---
nginx php 配置好后网页输出空白

查看了 nginx 和 php-fpm 的日志文件，都没有发现错误。实在是奇怪。

factcgi_params
搜索后解决了问题：在 location ~ \.php$ {} 配置里加上如下一行：

fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
