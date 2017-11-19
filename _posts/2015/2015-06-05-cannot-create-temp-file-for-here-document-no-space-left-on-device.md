---
layout: post
title: cannot create temp file for here document: No space left on device
date: 2015-06-05 13:42
author: admin
comments: true
categories: []
---
cannot create temp file for here document: No space left on device

磁盘空间满了

1. df -h

查哪个盘满了

 2. find  /  -size +1000000

找出过大文件，一般日志居多

3.删除无用文件
