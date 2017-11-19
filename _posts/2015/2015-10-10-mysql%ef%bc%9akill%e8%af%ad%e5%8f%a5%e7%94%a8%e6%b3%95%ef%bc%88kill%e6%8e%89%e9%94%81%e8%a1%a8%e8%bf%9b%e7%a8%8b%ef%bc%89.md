---
layout: post
title: MySQl：kill语句用法（kill掉锁表进程）
date: 2015-10-10 10:24
author: admin
comments: true
categories: []
---
<div><b><span style="color: #ff0000;">第一季</span></b></div>
手册确实需要经常看，今天又长了知识。MySQL的KILL命令不只可以杀掉连接，而且可以只杀掉某连接当前的SQL，而不断开连接。
KILL QUERY thread_id;
<div><span style="font-family: song, Verdana;"> </span></div>
<div><span style="font-family: song, Verdana;"> </span></div>
<div>
<table border="0" width="500" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="480">
<pre>KILL thread_id 可以杀掉当前的连接.KILL QUERY thread_id 不能杀掉.</pre>
</td>
</tr>
</tbody>
</table>
<b><span style="color: #ff0000;">第二季</span></b><wbr /><wbr /></div>
<div>　　KILL [CONNECTION | QUERY] thread_id

每个与mysqld的连接都在一个独立的线程里运行，您可以使用SHOW PROCESSLIST语句查看哪些线程正在运行，并使用KILL thread_id语句终止一个线程。

KILL允许自选的CONNECTION或QUERY修改符：

· KILL CONNECTION与不含修改符的KILL一样：它会终止与给定的thread_id有关的连接。

· KILL QUERY会终止连接当前正在执行的语句，但是会保持连接的原状。

如果您拥有PROCESS权限，则您可以查看所有线程。如果您拥有SUPER权限，您可以终止所有线程和语句。否则，您只能查看和终止您自己的线程和语句。

您也可以使用mysqladmin processlist和mysqladmin kill命令来检查和终止线程。

注释：您不能同时使用KILL和Embedded MySQL Server库，因为内植的服务器只运行主机应用程序的线程。它不能创建任何自身的连接线程。

当您进行一个KILL时，对线程设置一个特有的终止标记。在多数情况下，线程终止可能要花一些时间，这是因为终止标记只会在在特定的间隔被检查：

· 在SELECT, ORDER BY和GROUP BY循环中，在读取一组行后检查标记。如果设置了终止标记，则该语句被放弃。

· 在ALTER TABLE过程中，在每组行从原来的表中被读取前，检查终止标记。如果设置了终止标记，则语句被放弃，临时表被删除。

· 在UPDATE或DELETE运行期间，在每个组读取之后以及每个已更行或已删除的行之后，检查终止标记。如果终止标记被设置，则该语句被放弃。注意，如果您正在使用事务，则变更不会被 回滚。

· GET_LOCK()会放弃和返回NULL。

· INSERT DELAYED线程会快速地刷新（插入）它在存储器中的所有的行，然后终止。

· 如果线程在表锁定管理程序中（状态：锁定），则表锁定被快速地放弃。

· 如果在写入调用中，线程正在等待空闲的磁盘空间，则写入被放弃，并伴随"disk full"错误消息。

· 警告：对MyISAM表终止一个REPAIR TABLE或OPTIMIZE TABLE操作会导致出现一个被损坏的没有用的表。对这样的表的任何读取或写入都会失败，直到您再次优化或修复它（不中断）。</div>
<div></div>
<div><b><span style="color: #ff0000;">第三季</span></b></div>
<div>

3点钟刚睡下, 4点多, 同事打电话告诉我用户数据库挂掉了. 我起床看一下进程列表.
<div>
<div>mysql&gt;show processlist;</div>
</div>
出来哗啦啦好几屏幕的, 没有一千也有几百条, 查询语句把表锁住了, 赶紧找出第一个Locked的thread_id, 在mysql的shell里面执行.
<div>
<div>mysql&gt;kill thread_id;</div>
</div>
kill掉第一个锁表的进程, 依然没有改善. 既然不改善, 咱们就想办法将所有锁表的进程kill掉吧, 简单的脚本如下.
<div>
<div>#!/bin/bash
mysql -u root -e "show processlist" | grep -i "Locked" &gt;&gt; locked_log.txt

for line in `cat locked_log.txt | awk '{print $1}'`
do
echo "kill $line;" &gt;&gt; kill_thread_id.sql
done</div>
</div>
现在kill_thread_id.sql的内容像这个样子
<div>
<div>kill 66402982;
kill 66402983;
kill 66402986;
kill 66402991;
.....</div>
</div>
好了, 我们在mysql的shell中执行, 就可以把所有锁表的进程杀死了.
<div>
<div>mysql&gt;source kill_thread_id.sql
<span style="color: #ff0000;">
<span style="color: #339966;">当然了, 也可以一行搞定
</span></span>
<div>
<div><span style="color: #339966;">for id in `mysqladmin processlist | grep -i locked | awk '{print $1}'`
do
mysqladmin kill ${id}
done</span></div>
</div>
</div>
</div>
</div>
