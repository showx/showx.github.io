---
layout: post
title: Group by order by神奇之笔！！！！
date: 2014-08-28 11:26
author: admin
comments: true
categories: []
---
GROUP BY 是分组查询, 一般 GROUP BY 是和 聚合函数配合使用,你可以想想

你用了GROUP BY 按  ITEM.ITEMNUM 这个字段分组,那其他字段内容不同,变成一对多又改如何显示呢,比如下面所示

A  B
1  abc
1  bcd
1  asdfg

select A,B from table group by A
你说这样查出来是什么结果,

A  B
    abc 
1  bcd
    asdfg

右边3条如何变成一条,所以需要用到聚合函数,比如

select A,count(B) 数量 from table group by A
这样的结果就是
A  数量
1   3

group by 有一个原则,就是 select 后面的所有列中,没有使用聚合函数的列,必须出现在 group by 后面

 

http://hi.baidu.com/jintianwuliao/blog/item/17c8e4c4f5f0cacd39db49b3.html 
ORDER BY的例子

 

SELECT column1, SUM(column2)

　　FROM "list-of-tables"

　　ORDER BY "column-list" [ASC | DESC]; 

　　[ ] = optional 　　ORDER BY是一个可选的子句，它允许你根据指定要order by的列来以上升或者下降的顺序来显示查询的结果。例如： 

　　ASC = Ascending Order – 这个是缺省的

　　DESC = Descending Order 

　　下面举个例子：

　　SELECT employee_id, dept, name, age, salary

　　FROM employee_info

　　WHERE dept = 'Sales'

　　ORDER BY salary; 

　　这条SQL语句将从employee_info表中列dept等于'Sales'选择employee_id,、dept、 name、 age和 salary，并且根据他们的salary按升序的顺序来列出检索结果。

　　如果你想对多列排序的话，那么在列与列之间要加上逗号，比如 ：

　　SELECT employee_id, dept, name, age, salary

　　FROM employee_info

　　WHERE dept = 'Sales'

　　ORDER BY salary, age DESC;

 

sql语句Group By用法一则
如果我们的需求变成是要算出每一间店 (store_name) 的营业额 (sales)，那怎么办呢？在这个情况下，我们要做到两件事：第一，我们对于 store_name 及 Sales 这两个栏位都要选出。第二，我们需要确认所有的 sales 都要依照各个 store_name 来分开算。这个语法为：   

SELECT "栏位1", SUM("栏位2") FROM "表格名" GROUP BY "栏位1"   

在我们的示范上，   

Store_Information 表格
store_name Sales Date 
Los Angeles $1500 Jan-05-1999 
San Diego $250 Jan-07-1999 
Los Angeles $300 Jan-08-1999 
Boston $700 Jan-08-1999 
我们就打入， SELECT store_name, SUM(Sales)    FROM Store_Information GROUP BY store_name   

结果:   

store_name SUM(Sales) 
Los Angeles $1800 
San Diego $250 
Boston $700 

当我们选不只一个栏位，且其中至少一个栏位有包含函数的运用时，我们就需要用到 GROUP BY 这个指令。在这个情况下，我们需要确定我们有 GROUP BY 所有其他的栏位。换句话说，除了有包括函数的栏位外，我 们都需要将其放在 GROUP BY 的子句中

SUM
返回表达式中所有值的和，或只返回 DISTINCT 值。SUM 只能用于数字列。空值将被忽略。

A. 在聚合和行聚合中使用 SUM
下列示例显示聚合函数和行聚合函数之间的区别。第一个示例显示只提供汇总数据的聚合函数，第二个示例显示提供详尽数据和汇总数据的行聚合函数。

USE pubs
GO
-- Aggregate functions
SELECT type, SUM(price), SUM(advance)
FROM titles
WHERE type LIKE '%cook'
GROUP BY type
ORDER BY type
GO

下面是结果集：

type                                                               
------------ -------------------------- -------------------------- 
mod_cook     22.98                      15,000.00                  
trad_cook    47.89                      19,000.00                  

(2 row(s) affected)

USE pubs
GO
-- Row aggregates
SELECT type, price, advance
FROM titles
WHERE type LIKE '%cook'
ORDER BY type
COMPUTE SUM(price), SUM(advance) BY type

下面是结果集：

type         price                      advance                    
------------ -------------------------- -------------------------- 
mod_cook     19.99                      0.00                       
mod_cook     2.99                       15,000.00                  

             sum
             ==========================
             22.98                      
                                        sum
                                        ==========================
                                        15,000.00                  

type         price                      advance                    
------------ -------------------------- -------------------------- 
trad_cook    20.95                      7,000.00                   
trad_cook    11.95                      4,000.00                   
trad_cook    14.99                      8,000.00                   

             sum
             ==========================
             47.89                      
                                        sum
                                        ==========================
                                        19,000.00                 

(7 row(s) affected)

B. 计算多列的组合计
下例计算每类书籍的价格和预付款总和。

USE pubs
GO
SELECT type, SUM(price), SUM(advance)
FROM titles
GROUP BY type
ORDER BY type
GO

下面是结果集：

type                                                               
------------ -------------------------- -------------------------- 
business     54.92                      25,125.00                  
mod_cook     22.98                      15,000.00                  
popular_comp 42.95                      15,000.00                  
psychology   67.52                      21,275.00                  
trad_cook    47.89                      19,000.00                  
UNDECIDED    (null)                     (null)                     

(6 row(s) affected)
COUNT返回组中项目的数量。COUNT(*) 返回组中项目的数量，这些项目包括 NULL 值和副本。COUNT(ALL expression) 对组中的每一行都计算 expression 并返回非空值的数量。COUNT(DISTINCT expression) 对组中的每一行都计算 expression 并返回唯一非空值的数量。示例A. 使用 COUNT 和 DISTINCT下面的示例查找作者所居住的不同城市的数量。USE pubs
GO
SELECT COUNT(DISTINCT city)
FROM authors
GO

下面是结果集：
----------- 
16          

(1 row(s) affected)

B. 使用 COUNT(*)
下面的查询查找图书和书名的总数：
USE pubs
GO
SELECT COUNT(*)
FROM titles
GO

下面是结果集：
----------- 
18          

(1 row(s) affected)

C. 与其它聚合函数一起使用 COUNT(*)
下面的示例显示可以与选择列表中的其它聚合函数结合使用的 COUNT(*)。
USE pubs
GO
SELECT COUNT(*), AVG(price)
FROM titles
WHERE advance > $1000
GO

下面是结果集：
----------- -------------------------- 
15          14.42                      

(1 row(s) affected)


DECLARE @t TABLE(Groups char(2),Item varchar(10),Color varchar(10),Quantity int)
INSERT @t SELECT 'aa','Table','Blue', 124
UNION ALL SELECT 'bb','Table','Red',  -23
UNION ALL SELECT 'bb','Cup'  ,'Green',-23
UNION ALL SELECT 'aa','Chair','Blue', 101
UNION ALL SELECT 'aa','Chair','Red',  -90

--汇总显示
SELECT Groups=CASE 
        WHEN GROUPING(Color)=0 THEN Groups
        WHEN GROUPING(Groups)=1 THEN '总计'
        ELSE '' END,
    Item=CASE 
        WHEN GROUPING(Color)=0 THEN Item
        WHEN GROUPING(Item)=1 AND GROUPING(Groups)=0 THEN Groups+' 合计'
        ELSE '' END,
    Color=CASE 
        WHEN GROUPING(Color)=0 THEN Color
        WHEN GROUPING(Color)=1 AND GROUPING(Item)=0 THEN Item+' 小计'
        ELSE '' END,
    Quantity=SUM(Quantity)
FROM @t
GROUP BY Groups,Item,Color WITH ROLLUP
/*--结果
Groups Item       Color           Quantity    
-------- ---------------- ---------------------- ----------- 
aa     Chair      Blue            101
aa     Chair      Red             -90
                 Chair 小计       11
aa     Table      Blue            124
                 Table 小计       124
       aa 合计                    135
bb     Cup        Green           -23
                  Cup 小计        -23
bb     Table      Red             -23
                 Table 小计       -23
       bb 合计                    -46
总计                              89
--*/

－－－－－－－－－－－－－－－－－－－－－－－－－－

if object_id('[tb]') is not null drop table [tb]
go
create table [tb]([部门] varchar(6),[电话] varchar(20),[金额] int)
insert [tb]
select '营业部',8001,20 union all
select '营业部',8002,30 union all
select '财务部',6001,10 union all
select '财务部',6003,100

select 
  isnull(部门,'总计') as 部门,
  isnull(电话,'小计') as 电话,
  sum(金额) as 金额
from tb
group by 部门,电话 with rollup --测试结果:
/*
部门     电话                   金额          
------ -------------------- ----------- 
财务部    6001                 10
财务部    6003                 100
财务部    小计                   110
营业部    8001                 20
营业部    8002                 30
营业部    小计                   50
总计     小计                   160

（所影响的行数为 7 行）

*/
分类汇总compute,compute by,with rollup,with cube使用示例 
if object_id('[tb]') is not null drop table [tb]
go
create table [tb]([部门] varchar(7),[电话] varchar(20),[金额] int)
insert [tb]
select '营业部',8001,20 union all
select '营业部',8002,30 union all
select '财务部',6001,10 union all
select '财务部',6003,100 union all
select '财务部2',6004,50 union all
select '财务部2',6004,100


--COMPUTE 示例
select * 
from tb 
order by [部门] 
compute sum([金额])
/*
部门      电话                   金额
------- -------------------- -----------
财务部     6001                 10
财务部     6003                 100
财务部2    6004                 50
财务部2    6004                 100
营业部     8001                 20
营业部     8002                 30

sum
-----------
310


(7 行受影响)

*/
--COMPUTE BY 示例
select * 
from tb 
order by [部门] 
compute sum([金额]) by [部门]
/*
部门      电话                   金额
------- -------------------- -----------
财务部     6001                 10
财务部     6003                 100

sum
-----------
110

部门      电话                   金额
------- -------------------- -----------
财务部2    6004                 50
财务部2    6004                 100

sum
-----------
150

部门      电话                   金额
------- -------------------- -----------
营业部     8001                 20
营业部     8002                 30

sum
-----------
50


(9 行受影响)
*/
--with rollup 示例
select 
  isnull(部门,'总计') as 部门,
  isnull(电话,'小计') as 电话,
  sum(金额) as 金额
from tb
group by 部门,电话 
with rollup
/*
部门      电话                   金额
------- -------------------- -----------
财务部     6001                 10
财务部     6003                 100
财务部     小计                   110
财务部2    6004                 150
财务部2    小计                   150
营业部     8001                 20
营业部     8002                 30
营业部     小计                   50
总计      小计                   310

(9 行受影响)
*/
--with cube 示例
select 
  isnull(部门,'总计') as 部门,
  isnull(电话,'小计') as 电话,
  sum(金额) as 金额
from tb
group by 部门,电话 
with cube
/*
部门      电话                   金额
------- -------------------- -----------
财务部     6001                 10
财务部     6003                 100
财务部     小计                   110
财务部2    6004                 150
财务部2    小计                   150
营业部     8001                 20
营业部     8002                 30
营业部     小计                   50
总计      小计                   310
总计      6001                 10
总计      6003                 100
总计      6004                 150
总计      8001                 20
总计      8002                 30

(14 行受影响)
*/
