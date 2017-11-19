---
layout: post
title: linux/ubuntu下的redis及PHP扩展phpredis安装与配置详细步骤参考
date: 2015-07-14 08:44
author: admin
comments: true
categories: []
---
<h3 id="redis安装">redis安装</h3>
<blockquote>wget http://download.redis.io/redis-stable.tar.gz
tar -zxvf redis-stable.tar.gz
cd redis-stable
make
make install</blockquote>
到此redis简单安装即完成。

redis 由四个可执行文件：redis-benchmark、redis-cli、redis-server、redis-stat、redis-check-dump 这几个文件，加上一个redis.conf就构成了整个redis的最终可用包。它们的作用如下：

redis-server：Redis服务器的daemon启动程序
redis-cli：Redis命令行操作工具。当然，你也可以用telnet根据其纯文本协议来操作
redis-benchmark：Redis性能测试工具，测试Redis在你的系统及你的配置下的读写性能
redis-stat：Redis状态检测工具，可以检测Redis当前状态参数及延迟状况
redis-check-dump:本地数据库检查工具

当执行make install后，即会将以上五个文件复制到/usr/local/bin/中，然后即可在任意位置启动redis.

备注：这里出现一个错误，使用root用户make编译时出错，后退出到普通用户后即顺利编译成功。
<h3 id="redis配置">redis配置</h3>
为了管理和使用方便，我们继续进行相关配置
<blockquote>mkdir /usr/local/redis/
cp -a /root/redis-stable/* /usr/local/redis/
mkdir /usr/local/redis/conf/
mkdir /usr/local/redis/var/</blockquote>
创建并编辑配置文件：
<blockquote>vim /usr/local/redis/conf/redis.conf</blockquote>
添加以下内容：
<blockquote>#是否作为守护进程运行
daemonize yes
#配置pid的存放路径及文件名，默认为当前路径下
pidfile /usr/local/redis/var/redis.pid
#Redis默认监听端口
port 6379
#客户端闲置多少秒后，断开连接
timeout 300
#日志显示级别
loglevel verbose
#指定日志输出的文件名，也可指定到标准输出端口
#logfile stdout
logfile /usr/local/redis/var/redis.log
#设置数据库的数量，默认连接的数据库是0，可以通过select N来连接不同的数据库
databases 16
#保存数据到disk的策略
#当有一条Keys数据被改变是，900秒刷新到disk一次
save 900 1
#当有10条Keys数据被改变时，300秒刷新到disk一次
save 300 10
#当有1w条keys数据被改变时，60秒刷新到disk一次
save 60 10000
#当dump  .rdb数据库的时候是否压缩数据对象
rdbcompression yes
#dump数据库的数据保存的文件名
dbfilename dump.rdb
#Redis的工作目录
dir /usr/local/redis/var/
###########  Replication #####################
#Redis的复制配置
# slaveof &lt;masterip&gt; &lt;masterport&gt;
# masterauth &lt;master-password&gt;

############## SECURITY ###########
# requirepass foobared

############### LIMITS ##############
#最大客户端连接数
# maxclients 128
#最大内存使用率
# maxmemory &lt;bytes&gt;

########## APPEND ONLY MODE #########
#是否开启日志功能
appendonly no
# 刷新日志到disk的规则
# appendfsync always
appendfsync everysec
# appendfsync no
################ VIRTUAL MEMORY ###########
#是否开启VM功能
vm-enabled no
# vm-enabled yes
vm-swap-file logs/redis.swap
vm-max-memory 0
vm-page-size 32
vm-pages 134217728
vm-max-threads 4
############# ADVANCED CONFIG ###############
glueoutputbuf yes
hash-max-zipmap-entries 64
hash-max-zipmap-value 512
#是否重置Hash表
activerehashing yes</blockquote>
<h3 id="redis服务开启">redis服务开启</h3>
<blockquote>/usr/local/redis/src/redis-server /usr/local/redis/conf/redis.conf
或
redis-server /etc/redis.conf</blockquote>
查看服务是否已拉起：
<blockquote>$netstat -anp | grep 6379
$ps -ef |grep redis-server</blockquote>
将Redis加入启动选项：
<blockquote>$gedit /etc/rc.local</blockquote>
添加：
<blockquote>/usr/local/redis/src/redis-server /usr/local/redis/conf/redis.conf</blockquote>
<h3 id="redis客户端连接验证">redis客户端连接验证</h3>
<blockquote>$ redis-cli
redis 127.0.0.1:6379&gt; select 01
OK
redis 127.0.0.1:6379[1]&gt; keys *
1) “gl_ma_1_s”
2) “u_onlineUser_1″
3) “u_onlineCountM_1″</blockquote>
<strong>清除</strong>
<blockquote>$ redis-cli
redis 127.0.0.1:6379&gt; flushDb
OK</blockquote>
<h3 id="安装PHP扩展phpredis">安装PHP扩展phpredis</h3>
Redis官网上推荐了5种PHP扩展： Predis 、 Phpredis 、Rediska 、RedisServer 、Redisent .
这里选择的是Phpredis
<blockquote>unzip owlient-phpredis-2.1.1-1-g90ecd17.zip
cd owlient-phpredis-2.1.1-1-g90ecd17
/usr/bin/phpize
./configure -with-php-config=/usr/bin/php-config
make &amp;&amp; make install</blockquote>
如提示找不到php-config，请自行查找其位置所在。如：
<blockquote>./configure -with-php-config=/usr/local/php/bin/php-config</blockquote>
php.ini中添加extension=redis.so，重启 php- fpm：
<blockquote>sudo gedit /usr/local/php/etc/php.ini

/etc/init.d/php-fpm restart</blockquote>
<strong>Redis的PHP扩展测试</strong>
<div class="codeText">
<div class="codeHead">
<div id="highlighter_851750" class="syntaxhighlighter  Brush">
<div class="lines">
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>1</code></td>
<td class="content"><code class="Brush plain">&lt;?php  </code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>2</code></td>
<td class="content"><code class="Brush variable">$redis</code> <code class="Brush plain">= </code><code class="Brush keyword">new</code> <code class="Brush plain">Redis();  </code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>3</code></td>
<td class="content"><code class="Brush variable">$redis</code><code class="Brush plain">-&gt;connect(</code><code class="Brush string">'127.0.0.1'</code><code class="Brush plain">,6379);  </code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>4</code></td>
<td class="content"><code class="Brush variable">$redis</code><code class="Brush plain">-&gt;set(</code><code class="Brush string">'foo'</code><code class="Brush plain">,</code><code class="Brush string">'This Is Test String！ '</code><code class="Brush plain">);  </code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>5</code></td>
<td class="content"><code class="Brush functions">echo</code> <code class="Brush variable">$redis</code><code class="Brush plain">-&gt;get(</code><code class="Brush string">'foo'</code><code class="Brush plain">);  </code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>6</code></td>
<td class="content"><code class="Brush plain">?&gt;</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
页面输输出：This Is Test String！  PHP连接Redis成功。

</div>
</div>
