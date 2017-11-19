---
layout: post
title: MongoDB authenticate failed. Please check your username and password from your m
date: 2015-04-01 21:26
author: admin
comments: true
categories: []
---
最近闲的下了个RockMongo来学习，发现一登陆就报这个错误：
 
MongoDB authenticate failed. Please check your username and password from your mongo administrator.
 
于是乎找到rockmongo/config.php
 
修改 $MONGO["servers"][$i]["mongo_auth"] = true;//enable mongo authentication?
 
将权限验证打开，而后在登录就可以了。
 
备注：原来mongodb默认是不需要验证就能使用的，出现之前的错误多半是你加上了权限认证的缘故。
