---
layout: post
title: SQL中truncate table和delete的区别
date: 2014-01-25 22:02
author: admin
comments: true
categories: []
---
TRUNCATE TABLE 在功能上与不带 Where 子句的 Delete 语句相同：二者均删除表中的全部行。但 TRUNCATE TABLE 比 Delete 速度快，且使用的系统和事务日志资源少。 

Delete 语句每次删除一行，并在事务日志中为所删除的每行记录一项。TRUNCATE TABLE 通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。 

TRUNCATE TABLE 删除表中的所有行，但表结构及其列、约束、索引等保持不变。新行标识所用的计数值重置为该列的种子。如果想保留标识计数值，请改用 Delete。 

对于由 FOREIGN KEY 约束引用的表，不能使用 TRUNCATE TABLE，而应使用不带 Where 子句的 Delete 语句。由于 TRUNCATE TABLE 不记录在日志中，所以它不能激活触发器。 

TRUNCATE TABLE 不能用于参与了索引视图的表。 

truncate,delete,drop的异同点：  
注意:这里说的delete是指不带where子句的delete语句 
  
相同点:truncate和不带where子句的delete, 以及drop都会删除表内的数据  

不同点:  
1.truncate和 delete只删除数据不删除表的结构(定义)  
  drop语句将删除表的结构被依赖的约束(constrain),触发器(trigger),索引(index); 依赖于该表的存储过程/函数将保留,但是变为invalid状态. 
  
2.delete语句是dml,这个操作会放到rollback segement中,事务提交之后才生效;如果有相应的trigger,执行的时候将被触发.  
  truncate,drop是ddl, 操作立即生效,原数据不放到rollback segment中,不能回滚. 操作不触发trigger. 

3.delete语句不影响表所占用的extent, 高水线(high w2atermark)保持原位置不动  
   显然drop语句将表所占用的空间全部释放  
   truncate 语句缺省情况下将空间释放到 minextents个 extent,除非使用reuse storage;   truncate会将高水线复位(回到最开始). 

4.速度,一般来说: drop> truncate > delete 

5.安全性:小心使用drop 和truncate,尤其没有备份的时候.否则哭都来不及 

使用上： 
想删除部分数据行用delete,注意带上where子句. 回滚段要足够大. 

想删除表,当然用drop 

想保留表而将所有数据删除. 如果和事务无关,用truncate即可. 如果和事务有关,或者想触发trigger,还是用delete. 

如果是整理表内部的碎片,可以用truncate跟上reuse stroage,再重新导入/插入数据
