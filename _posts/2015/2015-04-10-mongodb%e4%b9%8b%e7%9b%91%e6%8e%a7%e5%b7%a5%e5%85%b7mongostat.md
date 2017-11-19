---
layout: post
title: mongoDB之监控工具mongostat
date: 2015-04-10 17:04
author: admin
comments: true
categories: []
---
mongostat是mongdb自带的状态检测工具，在命令行下使用。它会间隔固定时间获取mongodb的当前运行状态，并输出。如果你发现数据库突然变慢或者有其他问题的话，你第一手的操作就考虑采用mongostat来查看mongo的状态。

mongostat命令格式，当然也可以加参数：
<div>在第一个例子中，mongostat将返回数据的每一秒，持续20秒。 mongostat收集数据的mongod实例上运行的本地主机接口端口27017。以下所有调用产生相同的行为：</div>
<div></div>
<div>mongostat -rowcount 20 1</div>
<div>mongostat -rowcount 20</div>
<div>mongostat -N 20 1</div>
<div>mongostat -N 20</div>
<div>在下面的例子中，mongostat返回的数据每5分钟（300秒），只要在程序运行。 mongostat收集数据的mongod实例上运行的本地主机接口端口27017。以下两种调用产生相同的行为。</div>
<div></div>
<div>mongostat - rowcount 0 300</div>
<div>mongostat -N 0 300</div>
<div>mongostat 300</div>
<div>在下面的例子中，mongostat返回的数据每5分钟一个小时（12次）。mongostat收集数据的mongod实例上运行的本地主机接口端口27017。以下两种调用产生相同的行为。</div>
<div></div>
<div>mongostat -rowcount 12 300</div>
<div>mongostat -N 12 300</div>
<div>在许多情况下，使用 -discover将帮助整组机器的状态，提供更完整的快照。如果Mongos的过程中，连接到一个片式集群上运行在本地机器上的端口27017，你可以使用下面的形式从群集中的所有成员返回统计：</div>
<div></div>
mongostat -discover

以上参考文档：http://cn.docs.mongodb.org/manual/reference/mongostat/

&nbsp;

主要详细说明一下各列的意义(也可以参考./mongostat --help)
<div>insert:     一秒内的插入数</div>
<div>query :     一秒内的查询数</div>
<div>update:     一秒内的更新数</div>
<div>delete:     一秒内的删除数</div>
<div>  10条简单的查询可能比一条复杂的查询速度还快, 所以数值的大小，意义并不大。</div>
<div>  但至少可以知道，现在是否在处理查询，是否在插入。</div>
<div>  如果是slave，数值前往往有一个*, 代表是replicate操作</div>
<div></div>
<div>getmore:    查询时游标(cursor)的getmore操作</div>
<div>  用处不大</div>
<div>    www.2cto.com</div>
<div>command:    一秒内执行的命令数</div>
<div>  比如批量插入，只认为是一条命令。 意义不大。</div>
<div>  如果是slave，会显示两个值, local|replicated，通过这两个数值的比较，或许可以看出点问题。</div>
<div></div>
<div>flushes:    一秒内flush的次数</div>
<div>  一般都是0，或者1，通过计算两个1之间的间隔时间，可以大致了解多长时间flush一次。</div>
<div>  flush开销是很大的，如果频繁的flush，可能就要找找原因了。</div>
<div></div>
<div>mapped:</div>
<div>vsize:</div>
<div>res:</div>
<div>  这个和你用top看到的一样，mapped, vsize一般不会有大的变动， res会慢慢的上升，如果res经常突然下降，去查查是否有别的程序狂吃内存。</div>
<div></div>
<div>faults:</div>
<div>  别被这个名字吓着，大压力下这个数值往往不为0。如果经常不为0，那就该加内存了。</div>
<div></div>
<div>locked:</div>
<div>  MongoDB就一把读写锁，这里指的是写锁所住的时间百分比。这个数值过大(经常超过10%)，那就是出状况了。</div>
<div></div>
<div>idx miss:</div>
<div>  非常重要的参数, 正常情况下，所有的查询都应该通过索引，也就是idx miss为0。如果这里数值较大，是不是缺少索引。</div>
<div></div>
<div>qr|qw: queue lengths for clients waiting (read|write)</div>
<div>ar|aw: active clients (read|write)</div>
<div>  如果这两个数值很大，那么就是DB被堵住了，DB的处理速度不及请求速度。</div>
<div>  看看是否有开销很大的慢查询。如果查询一切正常，确实是负载很大，就需要加机器了。</div>
<div></div>
<div>netIn: network traffic in - bits</div>
<div>netOut: network traffic out - bits</div>
<div>  网络带宽压力，一般MongoDB，网络不会成为瓶颈</div>
<div></div>
<div>conn: number of open connections</div>
<div>  MongoDB为每一个连接创建一个线程，线程的创建和释放也是有开销的。尽量不要让这个数值很大。</div>
<div></div>
<div>repl: 服务器当前状态</div>
<div>    M   - master</div>
<div>    SEC - secondary</div>
<div>    REC - recovering</div>
<div>    UNK - unknown</div>
<div>    SLV - slave</div>
<div></div>
<div>time: 当前时间</div>
如果在windows下的cmd窗口中执行mongostat命令时，可能由于窗口太窄，监控数据排列较乱而阻碍视觉的情况，大家可以把结果输出到一个txt文件中，然后去查看这个文件，办法是曲折了一些哈哈。

E:\mongodb-win32-x86_64-2.2.1\bin\mongostat -n 2 &gt; E:\test.txt

打印2行结果到E盘的跟目录下的test.txt中。

参考文档：http://cn.docs.mongodb.org/manual/reference/mongostat/
