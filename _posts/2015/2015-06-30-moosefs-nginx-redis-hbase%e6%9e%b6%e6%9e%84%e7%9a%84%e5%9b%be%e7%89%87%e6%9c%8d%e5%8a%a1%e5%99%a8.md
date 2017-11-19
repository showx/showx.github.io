---
layout: post
title: Moosefs, Nginx, Redis, HBase架构的图片服务器
date: 2015-06-30 16:14
author: admin
comments: true
categories: []
---
<img src="http://img.ph.126.net/Wx7YyWIGTF0Gsc9G75OW_g==/28147497689088412.jpg" alt="Moosefs, Nginx, Redis, HBase架构的图片服务器 - no9.im - no.9" />


<strong>架构介绍
</strong>  1.负载均衡：HAproxy采用RoundRobin负载均衡算法，分载前端用户请求的压力到每个web图片服务器上，
2.web服务：采用Nginx-0.9.6 做图片的web服务器，对网站的大、中、小图片进行读取，加上<a href="http://wiki.nginx.org/HttpRedis" target="_blank" rel="nofollow">Nginx的Redis模块</a>对缓存中的微型(头像)图片进行读取，
3. 缓存服务器：存储网站的 微型图片，签名照，小头像，表情图片，通过Nginx的Redis模块直接读取，通过调用Redis的java API程序对数据进行写入，
4.存储单元：采用Moosefs 存储  大、中、小图片，并且提供监控管理界面，查看存储空间运行状态，
5.图片索引：将图片名和图片url路径作为键值对(Key/Value)，放入HBase 中存储，并且进行数据查询，避免图片重复存储，便于将来管理，
6.应用服务器：对图片写入的操作全部由Java应用服务器完成。
