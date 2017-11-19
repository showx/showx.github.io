---
layout: post
title: ucenter的uc_client的缓存更新
date: 2013-12-23 16:37
author: admin
comments: true
categories: []
---
由于有些应用使用ucenter并没有后台功能来更新uc_client的缓存。
主要引用client的文件，调用函数:
$aa = call_user_func(UC_API_FUNC, 'cache', 'update', array());
这样就可以更新缓存，避免缓存引发其它问题。
