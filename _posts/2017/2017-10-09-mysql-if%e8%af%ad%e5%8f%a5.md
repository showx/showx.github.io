---
layout: post
title: mysql if语句
date: 2017-10-09 16:04
author: admin
comments: true
categories: [Uncategorized]
---
select sum(if(gs=3,0,gmoney)) as gmoney from bone_soft_official_day limit 10;

使用这样，不用在语言里麻烦处理
