---
layout: post
title: mysqldump导出远程数据库
date: 2013-12-04 16:53
author: admin
comments: true
categories: []
---
mysqldump --opt -h192.168.0.156 -uUsername -pPassword databaseName>database.sql  || --all-databases
