---
layout: post
title: mysql PARTITION表分区要注意事项
date: 2017-04-18 16:33
author: admin
comments: true
categories: [Uncategorized]
---
把表进行分区，数据量有一定的时候，会减轻压力。

1.主键，一定要使用复合主键。例如，主键为id, 复合字段就是 id,date

2.按月进行分区，其它没区分出的，放在MAXVALUE

3.写个shell crond,每个月设置一次。

例:

alter table day PARTITION by range(itime_d div 100)
(
partition day_2017_01 values less than (201701),
partition day_2017_02 values less than (201702),
partition day_2017_03 values less than (201703),
PARTITION day_2017_04 VALUES less than MAXVALUE
);
