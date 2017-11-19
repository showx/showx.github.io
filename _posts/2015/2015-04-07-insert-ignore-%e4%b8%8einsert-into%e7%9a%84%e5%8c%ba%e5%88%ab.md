---
layout: post
title: INSERT IGNORE 与INSERT INTO的区别
date: 2015-04-07 14:49
author: admin
comments: true
categories: []
---
INSERT IGNORE 与INSERT INTO的区别就是INSERT IGNORE会忽略数据库中已经存在 的数据，如果数据库没有数据，就插入新的数据，如果有数据的话就跳过这条数据。这样就可以保留数据库中已经存在数据，达到在间隙中插入数据的目的。
eg:
insert ignore into table(name)  select  name from table2
