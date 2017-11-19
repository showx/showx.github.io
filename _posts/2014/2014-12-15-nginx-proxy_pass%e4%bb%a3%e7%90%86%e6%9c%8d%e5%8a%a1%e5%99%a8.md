---
layout: post
title: nginx proxy_pass代理服务器
date: 2014-12-15 15:37
author: admin
comments: true
categories: []
---
数据迁到新服务器上，不小心删了旧服务器的数据
而域名解析还没有生效，这时候只能用代理。
在旧的网站服务器nginx加上：
location / { 
      proxy_pass http://新服务器ip/;
} 
新的服务器nginx只能绑定ip就行了。
这样地址就先解析去新IP那里了。
