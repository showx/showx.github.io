---
layout: post
title: APACHE 一个虚拟主机两个servername
date: 2013-12-19 11:30
author: admin
comments: true
categories: []
---
<VirtualHost ip:80> 
ServerAdmin webmaster@localhost 
DocumentRoot /usr/local/www/html/domain.com 
ServerName domain.com 
ServerAlias domain.com www.domain.com 
</VirtualHost>
