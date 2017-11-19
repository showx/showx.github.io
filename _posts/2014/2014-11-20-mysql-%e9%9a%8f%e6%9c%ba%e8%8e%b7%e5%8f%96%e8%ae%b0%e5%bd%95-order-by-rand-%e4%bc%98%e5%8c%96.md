---
layout: post
title: mysql 随机获取记录 order by rand 优化
date: 2014-11-20 13:33
author: admin
comments: true
categories: []
---
如果要随机获取记录数，在mysql里最简单的方法肯定是order by rand()了，但是这种方法只能在表记录极少的情况下才能使用。主要是因为order by rand()导致了using filesort.这个时候查询类型会变成all，索引会失效。只需简单的变通下，完成可以做到同样的效果。 

根据记录的类型，分类连续和非连续两种。 
连续指记录是连续存放的，并且有字段可以证明记录是连续的，例如自增id。 
非连续是指记录是随机存放的，例如有条件的查询，结果肯定不是连续的。 

一、连续记录优化 
先得到表的最大id和最小id。select max(id),min(id) from table 

1.在程序里随机一个在最大id和最小id的中间数，查询的时候大于这个随机数的就是随机记录了。
Sql代码  收藏代码
select * from table where id > 中间数 limit length;  
缺点：如果中间数很大的话，获取不了需要的记录数，随机性不强 

2.在程序里随机n个最大id和最小id的中间数，查询的时候用in获得这几个中间数的记录
Sql代码  收藏代码
select * from table where id in (中间数1, 中间数2,中间数3)  
需要注意的是，如果你要获取5条记录，那建议随机10个数。 
缺点：性能不如第1种方法，但是随机性更强 

二、非连续记录优化 
其实非连续记录的方法一样可以应用在连续记录中。 
首先获得记录的总数，例如：select count(*) from table where groupid = 1; 
然后在程序里随机n个小于记录总数的中间数，之后通过循环
Sql代码  收藏代码
select * from table where groupid = 1 limit 中间数,1  
来获得记录。 
关于优化循环sql可以采用prepare或者union all来优化循环执行 

2009-3-1 添加 
这两天加班，所以只有了想法，并没有去求证。 
关于第三种方法利用limit达到随机的效果，我拿了点数据测试。 

总记录：175,410   条件记录：20,946 
order by rand
Sql代码  收藏代码
SELECT * FROM Member WHERE Country = "HK" ORDER BY RAND() limit 30  

limit 
Sql代码  收藏代码
SELECT * FROM Member WHERE Country = "HK" limit ?, 1  

多次运行，使用order by rand胜出，limit法慢主要是因为limit偏移量大的时候。 

所以，适当limit减低偏移量和增大数量可以有效提高性能，可以快过order by rand。 

最后，跟大家说声对不起，没测试过就胡乱说话。 

这也许只能作为其中一种思路，根据具体情况具体分析。 

附上我的测试程序
Php代码  收藏代码
$t = microtime(true);  
  
$dbh->fetchAll('SELECT * FROM Member WHERE Country = "HK" ORDER BY RAND() limit 30');  
echo microtime(true) - $t, '<br/>';  
  
$t = microtime(true);  
$count = $dbh->fetchField('SELECT COUNT(*) FROM Member WHERE Country = "HK"') / 1.5;  
  
$sth = $dbh->prepare('SELECT * FROM Member WHERE Country = "HK" limit ?, 3') ;  
  
for ($n = 0; $n < 10; $n++) {  
    $sth->bindParam(1, mt_rand(0, $count), PDO::PARAM_INT);  
    $sth->execute();  
    $sth->fetchAll();  
}  
  
echo microtime(true) - $t;exit;  
