---
layout: post
title:  mysql中：单表distinct、多表group by查询去除重复记录
date: 2014-08-28 11:24
author: admin
comments: true
categories: []
---
单表的唯一查询用：distinct

多表的唯一查询用：group by

distinct 查询多表时，left join 还有效，全连接无效，

在使用mysql时，有时需要查询出某个字段不重复的记录，虽然mysql提供有distinct这个关键字来过滤掉多余的重复记录只保留一条，但往往只用它来返回不重复记录的条数，而不是用它来返回不重复记录的所有值。其原因是distinct只能返回它的目标字段，而无法返回其它字段，用distinct不能解决的话，我只有用二重循环查询来解决，而这样对于一个数据量非常大的站来说，无疑是会直接影响到效率的。
下面先来看看例子：

表的结构如下：
id name
1 a
2 b
3 c
4 c
5 b

基本的表的结构大概这样，这只是一个简单的例子，实际的多表查询等等情况会复杂得多。

比如我想用一条语句查询得到name不重复的所有数据，那就必须使用distinct去掉多余的重复记录。

select distinct name from table
得到的结果是:

name
a
b
c

好像达到效果了，可是，我想要得到的是id值呢？改一下查询语句吧:

select distinct name, id from table

结果会是:

id name
1 a
2 b
3 c
4 c
5 b

distinct怎么没起作用？作用其实是起了，不过他同时作用了两个字段，也就是必须得id与name都相同的才会被排除。

我们再改改查询语句:

select id, distinct name from table

很遗憾，除了错误信息你什么也得不到，distinct必须放在开头。难到不能把distinct放到where条件里？试试，照样报错。

 

试了半天其他能想到的方法也不行，最后在mysql手册里找到一个用法，用group_concat(distinct name)配合group by name实现了我所需要的功能，兴奋，天佑我也，赶快试试。

报错,郁闷!

连mysql手册也跟我过不去，先给了我希望，然后又把我推向失望。

再仔细一查，group_concat函数是4.1支持，晕，我4.0的。没办法，升级，升完级一试，成功。

终于搞定了，不过这样一来，又必须要求客户也升级了。

突然灵机一闪，既然可以使用group_concat函数，那其它函数能行吗？

赶紧用count函数一试，成功，费了这么多工夫,原来就这么简单。

现在将完整语句放出:

select *, count(distinct name) from table group by name

结果:

id name count(distinct name)
1 a 1
2 b 1
3 c 1

最后一项是多余的，不用管就行了，目的达到。

原来mysql这么笨，轻轻一下就把他骗过去了，现在拿出来希望大家不要被这问题折腾。

再顺便说一句，group by 必须放在 order by 和 limit之前，不然会报错。

说一下group by的实际例子：

$sql = 'select DISTINCT n.nid,tn.tid,n.title,n.created,ni.thumbpath from {term_node} tn INNER JOIN {node} n ON n.nid=tn.nid INNER JOIN {node_images} ni ON ni.nid=n.nid where tn.tid IN('.implode(',', $tids).') ORDER BY n.nid DESC';
$res = db_query($sql);
$t_data = array();

while($r = db_fetch_array($res)) {

print_r($r);
}

用这个查询语句的时候,总会出现两个相同nid的情况，比如下面的结果

Array
(
[created] => 1215331278
[nid] => 1603
[tid] => 32
[title] => 夏日婚礼绿色沁饮DIY
[thumbpath] => files/node_images/home-77.1_tn.jpg
)
Array
(
[created] => 1215331278
[nid] => 1603
[tid] => 32
[title] => 夏日婚礼绿色沁饮DIY
[thumbpath] => files/node_images/003_primary_tn.jpg
)

上面用了DISTINCT也不管用，其实是管用了，但是我想查询结构里nid是唯一的。

最后用了group by

$sql = 'select
n.nid,tn.tid,n.title,n.created,ni.thumbpath from {term_node} tn INNER
JOIN {node} n ON n.nid=tn.nid INNER JOIN {node_images} ni ON
ni.nid=n.nid where tn.tid IN('.implode(',', $tids).') GROUP BY
n.nid DESC';
$res = db_query($sql);
$t_data = array();

while($r = db_fetch_array($res)) {

print_r($r);
}

我就得到了nid是唯一的。
