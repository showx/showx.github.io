---
layout: post
title: nginx: [emerg] duplicate listen options for 0.0.0.0:80
date: 2014-09-25 20:13
author: admin
comments: true
categories: []
---
I have a Rails app up and running on my server and now I'd like to add another one.

I want Nginx to check what the request is for and split traffic based on domain name

Both sites have their own nginx.conf symlinked into sites-enabled, but I get an error starting nginx Starting nginx: nginx: [emerg] duplicate listen options for 0.0.0.0:80 in /etc/nginx/sites-enabled/bubbles:6


===========

The default_server parameter, if present, will cause the server to become the default server for the specified address:port pair.

It's also obvious, there can be only one default server.

And it is also says:

A listen directive can have several additional parameters specific to socket-related system calls. They can be specified in any listen directive, but only once for the given address:port pair.

So, you should remove default and deferred from one of the listen 80 directives. And same applies to ipv6only=on directive as well.

指定端口80设置去掉默认服务器即可。
