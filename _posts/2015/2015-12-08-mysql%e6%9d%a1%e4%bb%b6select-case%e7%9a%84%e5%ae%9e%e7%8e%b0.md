---
layout: post
title: MySQL条件select case的实现
date: 2015-12-08 10:03
author: admin
comments: true
categories: []
---
2010-03-17 09:20select *,if(sva=1,"男","女") as ssva from tableame where id =1Quote控制流程函数

CASE value WHEN [compare-value] THEN result [WHEN [compare-value] THEN result ...] [ELSE result] END CASE WHEN [condition] THEN result [WHEN [condition] THEN result ...] [ELSE result] END

在第一个方案的返回结果中， value=compare-value。而第二个方案的返回结果是第一种情况的真实结果。如果没有匹配的结果值，则返回结果为ELSE后的结果，如果没有ELSE 部分，则返回值为 NULL。
<ol class="dp-xml">
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT CASE 1 WHEN 1 THEN 'one'</li>
	<li>-<span class="tag">&gt;</span> WHEN 2 THEN 'two' ELSE 'more' END;</li>
	<li class="alt">-<span class="tag">&gt;</span> 'one'</li>
	<li>MySQL<span class="tag">&gt;</span> SELECT CASE WHEN 1<span class="tag">&gt;</span>0 THEN 'true' ELSE 'false' END;</li>
	<li class="alt">-<span class="tag">&gt;</span> 'true'</li>
	<li>MySQL<span class="tag">&gt;</span> SELECT CASE BINARY 'B'</li>
	<li class="alt">-<span class="tag">&gt;</span> WHEN 'a' THEN 1 WHEN 'b' THEN 2 END;</li>
	<li>-<span class="tag">&gt;</span> NULL</li>
</ol>
一个CASE表达式的默认返回值类型是任何返回值的相容集合类型，但具体情况视其所在语境而定。如果用在字符串语境中，则返回结果味字符串。如果用在数字语境中，则返回结果为十进制值、实值或整数值。

IF(expr1,expr2,expr3) 如果 expr1 是TRUE (expr1 &lt;&gt; 0 and expr1 &lt;&gt; NULL)，则 IF()的返回值为expr2; 否则返回值则为 expr3。IF() 的返回值为数字值或字符串值，具体情况视其所在语境而定。
<ol class="dp-xml">
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IF(1<span class="tag">&gt;</span>2,2,3);</li>
	<li>-<span class="tag">&gt;</span> 3</li>
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IF(1<span class="tag">&lt;</span><span class="tag-name">2</span>,'yes ','no');</li>
	<li>-<span class="tag">&gt;</span> 'yes'</li>
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IF(STRCMP('test','test1'),'no','yes');</li>
	<li>-<span class="tag">&gt;</span> 'no'</li>
</ol>
如果expr2 或expr3中只有一个明确是 NULL，则IF() 函数的结果类型 为非NULL表达式的结果类型。

expr1 作为一个整数值进行计算，就是说，假如你正在验证浮点值或字符串值， 那么应该使用比较运算进行检验。
<ol class="dp-xml">
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IF(0.1,1,0);</li>
	<li>-<span class="tag">&gt;</span> 0</li>
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IF(0.1<span class="tag">&lt;</span><span class="tag">&gt;</span>0,1,0);</li>
	<li>-<span class="tag">&gt;</span> 1</li>
</ol>
在所示的第一个例子中，IF(0.1)的返回值为0，原因是 0.1 被转化为整数值，从而引起一个对 IF(0)的检验。这或许不是你想要的情况。在第二个例子中，比较检验了原始浮点值，目的是为了了解是否其为非零值。比较结果使用整数。

MySQL 条件select case：IF() (这一点在其被储存到临时表时很重要 ) 的默认返回值类型按照以下方式计算：

表达式

返回值

expr2 或expr3 返回值为一个字符串。

字符串

expr2 或expr3 返回值为一个浮点值。

浮点

expr2 或 expr3 返回值为一个整数。

整数

假如expr2 和expr3 都是字符串，且其中任何一个字符串区分大小写，则返回结果是区分大小写。

IFNULL(expr1,expr2)

假如expr1 不为 NULL，则 IFNULL() 的返回值为 expr1; 否则其返回值为 expr2。IFNULL()的返回值是数字或是字符串，具体情况取决于其所使用的语境。
<ol class="dp-xml">
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IFNULL(1,0);</li>
	<li>-<span class="tag">&gt;</span> 1</li>
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IFNULL(NULL,10);</li>
	<li>-<span class="tag">&gt;</span> 10</li>
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IFNULL(1/0,10);</li>
	<li>-<span class="tag">&gt;</span> 10</li>
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT IFNULL(1/0,'yes');</li>
	<li>-<span class="tag">&gt;</span> 'yes'</li>
</ol>
IFNULL(expr1,expr2)的默认结果值为两个表达式中更加“通用”的一个，顺序为STRING、 REAL或 INTEGER。假设一个基于表达式的表的情况， 或MySQL必须在内存储器中储存一个临时表中IFNULL()的返回值：

CREATE TABLE tmp SELECT IFNULL(1,'test') AS test；

在这个例子中，测试列的类型为 CHAR(4)。

NULLIF(expr1,expr2)

如果expr1 = expr2 成立，那么返回值为NULL，否则返回值为 expr1。这和CASE WHEN expr1 = expr2 THEN NULL ELSE expr1 END相同。
<ol class="dp-xml">
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT NULLIF(1,1);</li>
	<li>-<span class="tag">&gt;</span> NULL</li>
	<li class="alt">MySQL<span class="tag">&gt;</span> SELECT NULLIF(1,2);</li>
	<li>-<span class="tag">&gt;</span> 1</li>
</ol>
注意，如果参数不相等，则 MySQL 两次求得的值为 expr1 ，上述的相关内容就是对MySQL 条件select case的描述，希望会给你带来一些帮助在此方面。
