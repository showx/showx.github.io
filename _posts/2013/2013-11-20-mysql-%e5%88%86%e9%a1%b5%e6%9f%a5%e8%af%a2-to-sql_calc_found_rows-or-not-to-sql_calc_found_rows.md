---
layout: post
title:  MySQL 分页查询: To SQL_CALC_FOUND_ROWS or not to SQL_CALC_FOUND_ROWS?
date: 2013-11-20 10:19
author: admin
comments: true
categories: []
---
When we optimize clients’ SQL queries I pretty often see a queries with SQL_CALC_FOUND_ROWS option used. Many people think, that it is faster to use this option than run two separate queries: one – to get a result set, another – to count total number of rows. In this post I’ll try to check, is this true or not and when it is better to run two separate queries.
tudou@Gyyx
当我们优化客户端SQL查询时，我经常看到使用SQL_CALC_FOUND_ROWS选项的查询.很多人认为，使用此选项要比运行两个单独的查询速度更快:第一条SQL获取结果集,第二条获取全集的总数(分页中我们经常这样使用).在这篇文章中我将检验使用SQL_CALC_FOUND_ROWS是否比运行两个单独的查询更好.
For my tests I’ve created following simple table:
创建示例表:
CREATE TABLE `count_test` (
  `a` int(10) NOT NULL auto_increment,
  `b` int(10) NOT NULL,
  `c` int(10) NOT NULL,
  `d` varchar(32) NOT NULL,
  PRIMARY KEY  (`a`),
  KEY `bc` (`b`,`c`)
) ENGINE=MyISAM
Test data has been created with following script (which creates 10M records):
向示例表中插入1000W行数据:
mysql_connect("127.0.0.1", "root");
mysql_select_db("test");

for ($i = 0; $i < 10000000; $i++) {
    $b = $i % 1000;
    mysql_query("INSERT INTO count_test SET b=$b, c=ROUND(RAND()*10), d=MD5($i)");
}
First of all, let's try to perform some query on this table using indexed column b in where clause:
首先,我们尝试一条使用了索引b的查询
mysql> SELECT SQL_NO_CACHE SQL_CALC_FOUND_ROWS * FROM count_test WHERE b = 555 ORDER BY c LIMIT 5;
Results with SQL_CALC_FOUND_ROWS are following: for each b value it takes 20-100 sec to execute uncached and 2-5 sec after warmup. Such difference could be explained by the I/O which required for this query - mysql accesses all 10k rows this query could produce without LIMIT clause.
使用SQL_CALC_FOUND_ROWS的结果如下:每个b列中的值需要20-100秒左右来加载到内存中,然后运算2-5秒得到结果.这和不使用LIMIT子句查询1W行数据的I/O消耗相当.
Let's check, how long it'd take if we'll try to use two separate queries:
我们再来检验下使用两条SQL语句的时长:
mysql> SELECT SQL_NO_CACHE * FROM count_test WHERE b = 666 ORDER BY c LIMIT 5;
The results are following: it takes 0.01-0.11 sec to run this query first time and 0.00-0.02 sec for all consecutive runs.
结果如下:第一次执行需要0.01-.011秒,其后相同的查询只须0.00-0.02秒(因为与此查询相关的索引被加载到了内存中,加快了数据的检索).
And now - we need too check how long our COUNT query would take:
接下来我们还需检验下执行COUNT查询的时长:
mysql> SELECT SQL_NO_CACHE count(*) FROM count_test WHERE b = 666;
Result is really impressive here: 0.00-0.04 sec for all runs.
结果很棒:只须0.00-0.04秒.
So, as we can see, total time for SELECT+COUNT (0.00-0.15 sec) is much less than execution time for original query (2-100 sec). Let's take a look at EXPLAINs
因此，我们可以看到，对于SELECT+ COUNT（0.00-0.15秒）的总时间比使用SQL_CALC_FOUND_ROWS的查询（2-100秒）的执行时间少得多。让我们来看看EXPLAIN的情况:
mysql> explain SELECT SQL_CALC_FOUND_ROWS * FROM count_test WHERE b = 999 ORDER BY c LIMIT 5;
+----+-------------+------------+------+---------------+------+---------+-------+-------+-------------+
| id | select_type | table      | type | possible_keys | key  | key_len | ref   | rows  | Extra       |
+----+-------------+------------+------+---------------+------+---------+-------+-------+-------------+
|  1 | SIMPLE      | count_test | ref  | bc            | bc   | 4       | const | 75327 | Using where |
+----+-------------+------------+------+---------------+------+---------+-------+-------+-------------+
1 row in set (0.00 sec)

mysql> explain SELECT SQL_NO_CACHE count(*) FROM count_test WHERE b = 666;
+----+-------------+------------+------+---------------+------+---------+-------+------+-------------+
| id | select_type | table      | type | possible_keys | key  | key_len | ref   | rows | Extra       |
+----+-------------+------------+------+---------------+------+---------+-------+------+-------------+
|  1 | SIMPLE      | count_test | ref  | bc            | bc   | 4       | const | 5479 | Using index |
+----+-------------+------------+------+---------------+------+---------+-------+------+-------------+
1 row in set (0.00 sec)
Here is why our count was much faster - MySQL accessed our table data when calculated result set size even when this was not needed (after the first 5 rows specified in LIMIT clause). With count(*) it used index scan inly which is much faster here.
这就是为什么使用COUNT如此快速:MySQL计算结果集大小时甚至访问了这条语句并不需要的数据(包括那些不在LIMIT 5 范围内的数据).
Just to be objective I've tried to perform this test without indexes (full scan) and with index on b column. Results were following:
为了更加客观,我试着测试没有索引（全扫描）和B列的索引。结果如下：
Full-scan:
7 seconds for SQL_CALC_FOUND_ROWS.
7+7 seconds in case when two queries used.
Filesort:
1.8 seconds for SQL_CALC_FOUND_ROWS.
1.8+0.05 seconds in case when two queries used.
So, obvious conclusion from this simple test is: when we have appropriate indexes for WHERE/ORDER clause in our query, it is much faster to use two separate queries instead of one with SQL_CALC_FOUND_ROWS.
因此，这个测试的结论是：当我们有适当的索引可以应用时，应当使用两个单独的查询，这要比SQL_CALC_FOUND_ROWS快得多。
