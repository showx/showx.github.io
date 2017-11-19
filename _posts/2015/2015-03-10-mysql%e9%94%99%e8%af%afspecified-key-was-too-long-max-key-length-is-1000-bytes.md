---
layout: post
title: MySQL错误“Specified key was too long; max key length is 1000 bytes”
date: 2015-03-10 17:58
author: admin
comments: true
categories: []
---
今天在为数据库中的某两个字段设置unique索引的时候，出现了Specified key was too long; max key length is 1000 bytes错误

经过查询才知道，是Mysql的字段设置的太长了，于是我把这两个字段的长度改了一下就好了。 

建立索引时，数据库计算key的长度是累加所有Index用到的字段的char长度后再按下面比例乘起来不能超过限定的key长度1000： 
latin1 = 1 byte = 1 character 
uft8 = 3 byte = 1 character 
gbk = 2 byte = 1 character 
举例能看得更明白些，以GBK为例： 
CREATE UNIQUE INDEX `unique_record` ON reports (`report_name`, `report_client`, `report_city`); 
其中report_name varchar(200), report_client varchar(200), report_city varchar(200) 
(200 + 200 +200) * 2 = 1200 > 1000，所有就会报1071错误，只要将report_city改为varchar(100)那么索引就能成功建立。 
如果表是UTF8字符集，那索引还是建立不了。
