---
layout: post
title: MYSQL中TIMESTAMP类型的默认值
date: 2015-01-04 15:20
author: admin
comments: true
categories: []
---
  
MYSQL中TIMESTAMP类型可以设定默认值，就像其他类型一样。
1、自动UPDATE 和INSERT 到当前的时间：
表：
———————————
Table   Create Table                                                                         
—— ————————————————————————————-
t1      CREATE TABLE `t1` (                                                                  
          `p_c` int(11) NOT NULL,                                                            
          `p_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
        ) ENGINE=InnoDB DEFAULT CHARSET=gb2312                                              
数据：
1    2007-10-08 11:53:35
2    2007-10-08 11:54:00
insert into t1(p_c) select 3;
update t1 set p_c = 2 where p_c = 5;
数据：
1    2007-10-08 11:53:35
5    2007-10-08 12:00:37
3    2007-10-08 12:00:37
2、自动INSERT 到当前时间，不过不自动UPDATE。
表：
———————————
Table   Create Table                                             
—— ———————————————————
t1      CREATE TABLE `t2` (                                      
          `p_c` int(11) NOT NULL,                                
          `p_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP 
        ) ENGINE=InnoDB DEFAULT CHARSET=gb2312                  
数据：
insert into t2(p_c) select 4;
update t2 set p_c = 3 where p_c = 5;
1    2007-10-08 11:53:35
2    2007-10-08 12:00:37
5    2007-10-08 12:00:37
4    2007-10-08 12:05:19
3、一个表中不能有两个字段默认值是当前时间，否则就会出错。不过其他的可以。
表：
———————————
Table   Create Table                                                   
—— —————————————————————
t1      CREATE TABLE `t1` (                                            
          `p_c` int(11) NOT NULL,                                      
          `p_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,       
          `p_timew2` timestamp NOT NULL DEFAULT '0000-00-00 00:00:00' 
        ) ENGINE=InnoDB DEFAULT CHARSET=gb2312                        
数据：
1    2007-10-08 11:53:35    0000-00-00 00:00:00
2    2007-10-08 12:00:37    0000-00-00 00:00:00
3    2007-10-08 12:00:37    0000-00-00 00:00:00
4    2007-10-08 12:05:19    0000-00-00 00:00:00
TIMESTAMP的变体
1，TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
在创建新记录和修改现有记录的时候都对这个数据列刷新

2，TIMESTAMP DEFAULT CURRENT_TIMESTAMP
在创建新记录的时候把这个字段设置为当前时间，但以后修改时，不再刷新它

3，TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
在创建新记录的时候把这个字段设置为0，以后修改时刷新它

4，TIMESTAMP DEFAULT ‘yyyy-mm-dd hh:mm:ss’ ON UPDATE CURRENT_TIMESTAMP 
在创建新记录的时候把这个字段设置为给定值，以后修改时刷新它
