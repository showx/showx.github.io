---
layout: post
title: Mongodb常用命令介绍
date: 2015-04-10 17:07
author: admin
comments: true
categories: []
---
查看命令的方式：

1.在shell中运行db.listCommands()

2.在浏览器中访问管理员接口：http://ipaddress:28017/_commands

&nbsp;

下面介绍在Mongodb中最经常使用的命令，具体如下：

命令：<strong>buildInfo</strong>

格式：{"buildInfo":1}

介绍：管理专用命令，返回Mongodb服务器的版本号和主机的操作系统。

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"buildInfo"</span>:<span class="number">1</span>})</li>
	<li>{</li>
	<li>    <span class="string">"version"</span> : <span class="string">"2.0.6"</span>,</li>
	<li>    <span class="string">"gitVersion"</span> : <span class="string">"e1c0cbc25863f6356aa4e31375add7bb49fb05bc"</span>,</li>
	<li>    <span class="string">"sysInfo"</span> : <span class="string">"Linux domU-12-31-39-01-70-B4 2.6.21.7-2.fc8xen #1 SMP Fri Feb 15 12:39:36 EST 2008 i686 BOOST_LIB_VERSION=1_41"</span>,</li>
	<li>    <span class="string">"versionArray"</span> : [</li>
	<li>        <span class="number">2</span>,</li>
	<li>        <span class="number">0</span>,</li>
	<li>        <span class="number">6</span>,</li>
	<li>        <span class="number">0</span></li>
	<li>    ],</li>
	<li>    <span class="string">"bits"</span> : <span class="number">32</span>,</li>
	<li>    <span class="string">"debug"</span> : false,</li>
	<li>    <span class="string">"maxBsonObjectSize"</span> : <span class="number">16777216</span>,</li>
	<li>    <span class="string">"ok"</span> : <span class="number">1</span></li>
	<li>}</li>
</ol>
</div>
命令：<strong>collStats</strong>

格式：{"collStats":collection}

介绍：返回指定集合的统计信息，包括数据大小、已分配的存储空间和索引的大小。

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"collStats"</span>:<span class="string">"users"</span>})</li>
	<li>{</li>
	<li>    <span class="string">"ns"</span> : <span class="string">"test.users"</span>,</li>
	<li>    <span class="string">"count"</span> : <span class="number">3</span>,</li>
	<li>    <span class="string">"size"</span> : <span class="number">508</span>,</li>
	<li>    <span class="string">"avgObjSize"</span> : <span class="number">169.33333333333334</span>,</li>
	<li>    <span class="string">"storageSize"</span> : <span class="number">4096</span>,</li>
	<li>    <span class="string">"numExtents"</span> : <span class="number">1</span>,</li>
	<li>    <span class="string">"nindexes"</span> : <span class="number">2</span>,</li>
	<li>    <span class="string">"lastExtentSize"</span> : <span class="number">4096</span>,</li>
	<li>    <span class="string">"paddingFactor"</span> : <span class="number">1.51</span>,</li>
	<li>    <span class="string">"flags"</span> : <span class="number">0</span>,</li>
	<li>    <span class="string">"totalIndexSize"</span> : <span class="number">16352</span>,</li>
	<li>    <span class="string">"indexSizes"</span> : {</li>
	<li>        <span class="string">"_id_"</span> : <span class="number">8176</span>,</li>
	<li>        <span class="string">"name_1"</span> : <span class="number">8176</span></li>
	<li>    },</li>
	<li>    <span class="string">"ok"</span> : <span class="number">1</span></li>
	<li>}</li>
</ol>
</div>
命令：<strong>distinct</strong>

格式：{"distinct":collection,"key":key,"query":query}

介绍：列出指定集合中满足查询条件的文档的指定键的所有不同值

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"distinct"</span>:<span class="string">"foo"</span>,<span class="string">"key"</span>:<span class="string">"name"</span>,<span class="string">"query"</span>:{<span class="string">"age"</span>:{<span class="string">"$gt"</span>:<span class="number">20</span>}}})</li>
	<li>{</li>
	<li>    <span class="string">"values"</span> : [</li>
	<li>        <span class="string">"gongyong"</span>,</li>
	<li>        <span class="string">"chenzhou"</span>,</li>
	<li>        <span class="string">"yixin"</span></li>
	<li>    ],</li>
	<li>    <span class="string">"stats"</span> : {</li>
	<li>        <span class="string">"n"</span> : <span class="number">4</span>,</li>
	<li>        <span class="string">"nscanned"</span> : <span class="number">6</span>,</li>
	<li>        <span class="string">"nscannedObjects"</span> : <span class="number">6</span>,</li>
	<li>        <span class="string">"timems"</span> : <span class="number">50</span>,</li>
	<li>        <span class="string">"cursor"</span> : <span class="string">"BasicCursor"</span></li>
	<li>    },</li>
	<li>    <span class="string">"ok"</span> : <span class="number">1</span></li>
	<li>}</li>
</ol>
</div>
命令：<strong>drop</strong>

格式：{"drop":collection}

介绍：删除集合的所有数据

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.bbb.save({<span class="string">"x"</span>:<span class="number">1</span>,<span class="string">"y"</span>:<span class="number">2</span>})</li>
	<li>#先往bbb中存一条记录</li>
	<li>&gt; db.bbb.find() #查询bbb中的数据</li>
	<li>{ <span class="string">"_id"</span> : ObjectId(<span class="string">"5027d919831a10b0f6e61385"</span>), <span class="string">"x"</span> : <span class="number">1</span>, <span class="string">"y"</span> : <span class="number">2</span> }</li>
	<li>#使用drop命令删除bbb集合中的数据</li>
	<li>&gt; db.runCommand({<span class="string">"drop"</span>:<span class="string">"bbb"</span>})</li>
	<li>{</li>
	<li>    <span class="string">"nIndexesWas"</span> : <span class="number">1</span>,</li>
	<li>    <span class="string">"msg"</span> : <span class="string">"indexes dropped for collection"</span>,</li>
	<li>    <span class="string">"ns"</span> : <span class="string">"test.bbb"</span>,</li>
	<li>    <span class="string">"ok"</span> : <span class="number">1</span></li>
	<li>}</li>
	<li>&gt; db.bbb.find() #再次查询，结果为空</li>
</ol>
</div>
命令：<strong>dropDatabase</strong>

格式：{"dropDatabase":1}

介绍：删除当前数据库中的所有数据

示例：略

&nbsp;

命令：<strong>dropIndexes</strong>

格式：{"dropIndexes":collection,"index":name}

介绍：删除集合里面名称为name的索引，如果名称为"*"，则删除全部索引。

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.system.indexes.find()</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.foo"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.users"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.games"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.blog.post"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.lists"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.math"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"name"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.users"</span>, <span class="string">"name"</span> : <span class="string">"name_1"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.map"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"gps"</span> : <span class="string">"2d"</span> }, <span class="string">"ns"</span> : <span class="string">"test.map"</span>, <span class="string">"name"</span> : <span class="string">"gps_"</span>, <span class="string">"min"</span> : -<span class="number">180</span>, <span class="string">"max"</span> : <span class="number">181</span> }</li>
	<li>&gt; db.runCommand({<span class="string">"dropIndexes"</span>:<span class="string">"users"</span>,<span class="string">"index"</span>:<span class="string">"name_1"</span>})</li>
	<li>{ <span class="string">"nIndexesWas"</span> : <span class="number">2</span>, <span class="string">"ok"</span> : <span class="number">1</span> }</li>
	<li>&gt; db.system.indexes.find()</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.foo"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.users"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.games"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.blog.post"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.lists"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.math"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"_id"</span> : <span class="number">1</span> }, <span class="string">"ns"</span> : <span class="string">"test.map"</span>, <span class="string">"name"</span> : <span class="string">"_id_"</span> }</li>
	<li>{ <span class="string">"v"</span> : <span class="number">1</span>, <span class="string">"key"</span> : { <span class="string">"gps"</span> : <span class="string">"2d"</span> }, <span class="string">"ns"</span> : <span class="string">"test.map"</span>, <span class="string">"name"</span> : <span class="string">"gps_"</span>, <span class="string">"min"</span> : -<span class="number">180</span>, <span class="string">"max"</span> : <span class="number">181</span> }</li>
</ol>
</div>
命令：findAndModify

格式：

介绍：查找并修改

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.foo.find({<span class="string">"name"</span>:<span class="string">"yixin"</span>})</li>
	<li>{ <span class="string">"_id"</span> : ObjectId(<span class="string">"5027d84b831a10b0f6e61384"</span>), <span class="string">"name"</span> : <span class="string">"yixin"</span>, <span class="string">"age"</span> : <span class="number">23</span> }</li>
	<li>&gt; db.runCommand({<span class="string">"findAndModify"</span>:<span class="string">"foo"</span>,</li>
	<li>... <span class="string">"query"</span>:{<span class="string">"name"</span>:<span class="string">"yixin"</span>},</li>
	<li>... <span class="string">"update"</span>:{<span class="string">"$inc"</span>:{<span class="string">"age"</span>:<span class="number">1</span>}}}) #把name为yixin的记录中age值加<span class="number">1</span></li>
	<li>{</li>
	<li>    <span class="string">"lastErrorObject"</span> : {</li>
	<li>        <span class="string">"updatedExisting"</span> : true,</li>
	<li>        <span class="string">"n"</span> : <span class="number">1</span>,</li>
	<li>        <span class="string">"connectionId"</span> : <span class="number">2</span>,</li>
	<li>        <span class="string">"err"</span> : null,</li>
	<li>        <span class="string">"ok"</span> : <span class="number">1</span></li>
	<li>    },</li>
	<li>    <span class="string">"value"</span> : {</li>
	<li>        <span class="string">"_id"</span> : ObjectId(<span class="string">"5027d84b831a10b0f6e61384"</span>),</li>
	<li>        <span class="string">"name"</span> : <span class="string">"yixin"</span>,</li>
	<li>        <span class="string">"age"</span> : <span class="number">23</span></li>
	<li>    },</li>
	<li>    <span class="string">"ok"</span> : <span class="number">1</span></li>
	<li>}</li>
	<li>&gt; db.foo.find({<span class="string">"name"</span>:<span class="string">"yixin"</span>})</li>
	<li>{ <span class="string">"_id"</span> : ObjectId(<span class="string">"5027d84b831a10b0f6e61384"</span>), <span class="string">"name"</span> : <span class="string">"yixin"</span>, <span class="string">"age"</span> : <span class="number">24</span> }</li>
</ol>
</div>
命令：<strong>getLastError</strong>

格式：{"getLastError":1[ , "w":w[ , "wtimeout":timeout}

介绍：查看对本集合执行的最后一次操作的错误信息或者其它状态信息。在w台服务器复制集合的最后操作之前，这个命令会阻塞。

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"getLastError"</span>:<span class="number">1</span>})</li>
	<li>{ <span class="string">"n"</span> : <span class="number">0</span>, <span class="string">"connectionId"</span> : <span class="number">2</span>, <span class="string">"err"</span> : null, <span class="string">"ok"</span> : <span class="number">1</span> }</li>
</ol>
</div>
命令：<strong>isMaster</strong>

格式：{"isMaster":1}

介绍：检查本服务器是主服务器还是从服务器

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"isMaster"</span>:<span class="number">1</span>})</li>
	<li>{ <span class="string">"ismaster"</span> : true, <span class="string">"maxBsonObjectSize"</span> : <span class="number">16777216</span>, <span class="string">"ok"</span> : <span class="number">1</span> }</li>
</ol>
</div>
命令：<strong>listCommands</strong>

格式：{"listCommands":1}

介绍：返回所有可以在服务器上运行的命令及相关信息。

示例： 返回结果太多，示例就省略了

&nbsp;

命令：<strong>listDatabases</strong>

格式：{"listDatabases":1}

介绍：管理专用命令，列出服务器上所有的数据库

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"listDatabases"</span>:<span class="number">1</span>})</li>
	<li>{ <span class="string">"errmsg"</span> : <span class="string">"access denied; use admin db"</span>, <span class="string">"ok"</span> : <span class="number">0</span> }</li>
	<li>#报错，提示访问被拒绝，要求必须使用admin数据库</li>
	<li>&gt; use admin #切换到admin数据库</li>
	<li>switched to db admin</li>
	<li>&gt; db.runCommand({<span class="string">"listDatabases"</span>:<span class="number">1</span>})</li>
	<li>{</li>
	<li>    <span class="string">"databases"</span> : [</li>
	<li>        {</li>
	<li>            <span class="string">"name"</span> : <span class="string">"test"</span>,</li>
	<li>            <span class="string">"sizeOnDisk"</span> : <span class="number">67108864</span>,</li>
	<li>            <span class="string">"empty"</span> : false</li>
	<li>        },</li>
	<li>        {</li>
	<li>            <span class="string">"name"</span> : <span class="string">"results"</span>,</li>
	<li>            <span class="string">"sizeOnDisk"</span> : <span class="number">67108864</span>,</li>
	<li>            <span class="string">"empty"</span> : false</li>
	<li>        },</li>
	<li>        {</li>
	<li>            <span class="string">"name"</span> : <span class="string">"admin"</span>,</li>
	<li>            <span class="string">"sizeOnDisk"</span> : <span class="number">1</span>,</li>
	<li>            <span class="string">"empty"</span> : true</li>
	<li>        },</li>
	<li>        {</li>
	<li>            <span class="string">"name"</span> : <span class="string">"local"</span>,</li>
	<li>            <span class="string">"sizeOnDisk"</span> : <span class="number">1</span>,</li>
	<li>            <span class="string">"empty"</span> : true</li>
	<li>        }</li>
	<li>    ],</li>
	<li>    <span class="string">"totalSize"</span> : <span class="number">134217728</span>,</li>
	<li>    <span class="string">"ok"</span> : <span class="number">1</span></li>
	<li>}</li>
</ol>
</div>
命令：<strong>ping</strong>

格式：{"ping":1}

介绍：检查服务器链接是否正常。即便服务器上锁了，这条命令也会立刻返回

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"ping"</span>:<span class="number">1</span>})</li>
	<li>{ <span class="string">"ok"</span> : <span class="number">1</span> }</li>
</ol>
</div>
命令：<strong>renameCollection</strong>

格式：{"renameCollection":a,"to":b}

介绍：将集合a重命名为b，其中a和b都必须是完整的集合命名空间（例如"test.foo"代表test数据库中的foo集合）

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"renameCollection"</span>:<span class="string">"foo"</span>,<span class="string">"to"</span>:<span class="string">"foo_bak"</span>})</li>
	<li>{ <span class="string">"errmsg"</span> : <span class="string">"access denied; use admin db"</span>, <span class="string">"ok"</span> : <span class="number">0</span> }</li>
	<li>&gt; use admin</li>
	<li>switched to db admin</li>
	<li>&gt; db.runCommand({<span class="string">"renameCollection"</span>:<span class="string">"foo"</span>,<span class="string">"to"</span>:<span class="string">"foo_bak"</span>})</li>
	<li>{</li>
	<li>    <span class="string">"errmsg"</span> : <span class="string">"exception: source namespace does not exist"</span>,</li>
	<li>    <span class="string">"code"</span> : <span class="number">10026</span>,</li>
	<li>    <span class="string">"ok"</span> : <span class="number">0</span></li>
	<li>}</li>
	<li>&gt; db.runCommand({<span class="string">"renameCollection"</span>:<span class="string">"test.foo"</span>,<span class="string">"to"</span>:<span class="string">"test.foo_bak"</span>})</li>
	<li>{ <span class="string">"ok"</span> : <span class="number">1</span> }</li>
	<li>&gt; db.foo_bak.find()</li>
	<li>&gt; use test</li>
	<li>switched to db test</li>
	<li>&gt; db.foo_bak.find()</li>
	<li>{ <span class="string">"_id"</span> : ObjectId(<span class="string">"502149203e2ff9961cc7b555"</span>), <span class="string">"name"</span> : <span class="string">"chenzhou"</span> }</li>
	<li>{ <span class="string">"_id"</span> : ObjectId(<span class="string">"502149353e2ff9961cc7b556"</span>), <span class="string">"age"</span> : <span class="number">23</span> }</li>
	<li>{ <span class="string">"_id"</span> : ObjectId(<span class="string">"5021495c3e2ff9961cc7b557"</span>), <span class="string">"name"</span> : <span class="string">"gongyong"</span>, <span class="string">"age"</span> : <span class="number">26</span> }</li>
	<li>{ <span class="string">"_id"</span> : ObjectId(<span class="string">"50214ad63e2ff9961cc7b558"</span>), <span class="string">"name"</span> : <span class="string">"chenzhou"</span>, <span class="string">"age"</span> : <span class="number">22</span>, <span class="string">"friends"</span> : <span class="number">500</span>, <span class="string">"enemies"</span> : <span class="number">2</span> }</li>
	<li>{ <span class="string">"_id"</span> : <span class="number">123</span>, <span class="string">"x"</span> : <span class="number">1</span> }</li>
	<li>{ <span class="string">"_id"</span> : ObjectId(<span class="string">"5027d84b831a10b0f6e61384"</span>), <span class="string">"name"</span> : <span class="string">"yixin"</span>, <span class="string">"age"</span> : <span class="number">24</span> }</li>
</ol>
</div>
命令：<strong>repairDatabase</strong>

格式：{"repairDatabase":1}

介绍：修复并压缩当前数据库，这个操作可能非常耗时。

示例：略

&nbsp;

命令：<strong>serverStatus</strong>

格式：{"serverStatus":1}

介绍：返回这台服务器的管理统计信息。

示例：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Shell代码 <embed src="http://chenzhou123520.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://chenzhou123520.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-default" start="1">
	<li>&gt; db.runCommand({<span class="string">"serverStatus"</span>:<span class="number">1</span>})</li>
	<li>{</li>
	<li>    <span class="string">"host"</span> : <span class="string">"localhost.localdomain"</span>,</li>
	<li>    <span class="string">"version"</span> : <span class="string">"2.0.6"</span>,</li>
	<li>    <span class="string">"process"</span> : <span class="string">"mongod"</span>,</li>
	<li>    <span class="string">"uptime"</span> : <span class="number">3661</span>,</li>
	<li>    <span class="string">"uptimeEstimate"</span> : <span class="number">2987</span>,</li>
	<li>    <span class="string">"localTime"</span> : ISODate(<span class="string">"2012-08-12T17:07:05.076Z"</span>),</li>
	<li>    <span class="string">"globalLock"</span> : {</li>
	<li>        <span class="string">"totalTime"</span> : <span class="number">3659420009</span>,</li>
	<li>        <span class="string">"lockTime"</span> : <span class="number">609397</span>,</li>
	<li>        <span class="string">"ratio"</span> : <span class="number">0.00016652830188971076</span>,</li>
	<li>        <span class="string">"currentQueue"</span> : {</li>
	<li>            <span class="string">"total"</span> : <span class="number">0</span>,</li>
	<li>            <span class="string">"readers"</span> : <span class="number">0</span>,</li>
	<li>            <span class="string">"writers"</span> : <span class="number">0</span></li>
	<li>        },</li>
	<li>        <span class="string">"activeClients"</span> : {</li>
	<li>            <span class="string">"total"</span> : <span class="number">0</span>,</li>
	<li>            <span class="string">"readers"</span> : <span class="number">0</span>,</li>
	<li>            <span class="string">"writers"</span> : <span class="number">0</span></li>
	<li>        }</li>
	<li>    },</li>
	<li>    <span class="string">"mem"</span> : {</li>
	<li>        <span class="string">"bits"</span> : <span class="number">32</span>,</li>
	<li>        <span class="string">"resident"</span> : <span class="number">45</span>,</li>
	<li>        <span class="string">"virtual"</span> : <span class="number">158</span>,</li>
	<li>        <span class="string">"supported"</span> : true,</li>
	<li>        <span class="string">"mapped"</span> : <span class="number">64</span></li>
	<li>    },</li>
	<li>    <span class="string">"connections"</span> : {</li>
	<li>        <span class="string">"current"</span> : <span class="number">1</span>,</li>
	<li>        <span class="string">"available"</span> : <span class="number">818</span></li>
	<li>    },</li>
	<li>    <span class="string">"extra_info"</span> : {</li>
	<li>        <span class="string">"note"</span> : <span class="string">"fields vary by platform"</span>,</li>
	<li>        <span class="string">"heap_usage_bytes"</span> : <span class="number">491848</span>,</li>
	<li>        <span class="string">"page_faults"</span> : <span class="number">0</span></li>
	<li>    },</li>
	<li>    <span class="string">"indexCounters"</span> : {</li>
	<li>        <span class="string">"btree"</span> : {</li>
	<li>            <span class="string">"accesses"</span> : <span class="number">1</span>,</li>
	<li>            <span class="string">"hits"</span> : <span class="number">1</span>,</li>
	<li>            <span class="string">"misses"</span> : <span class="number">0</span>,</li>
	<li>            <span class="string">"resets"</span> : <span class="number">0</span>,</li>
	<li>            <span class="string">"missRatio"</span> : <span class="number">0</span></li>
	<li>        }</li>
	<li>    },</li>
	<li>    <span class="string">"backgroundFlushing"</span> : {</li>
	<li>        <span class="string">"flushes"</span> : <span class="number">60</span>,</li>
	<li>        <span class="string">"total_ms"</span> : <span class="number">119</span>,</li>
	<li>        <span class="string">"average_ms"</span> : <span class="number">1.9833333333333334</span>,</li>
	<li>        <span class="string">"last_ms"</span> : <span class="number">0</span>,</li>
	<li>        <span class="string">"last_finished"</span> : ISODate(<span class="string">"2012-08-12T17:06:05.816Z"</span>)</li>
	<li>    },</li>
	<li>    <span class="string">"cursors"</span> : {</li>
	<li>        <span class="string">"totalOpen"</span> : <span class="number">0</span>,</li>
	<li>        <span class="string">"clientCursors_size"</span> : <span class="number">0</span>,</li>
	<li>        <span class="string">"timedOut"</span> : <span class="number">0</span></li>
	<li>    },</li>
	<li>    <span class="string">"network"</span> : {</li>
	<li>        <span class="string">"bytesIn"</span> : <span class="number">7312</span>,</li>
	<li>        <span class="string">"bytesOut"</span> : <span class="number">63515</span>,</li>
	<li>        <span class="string">"numRequests"</span> : <span class="number">102</span></li>
	<li>    },</li>
	<li>    <span class="string">"opcounters"</span> : {</li>
	<li>        <span class="string">"insert"</span> : <span class="number">2</span>,</li>
	<li>        <span class="string">"query"</span> : <span class="number">29</span>,</li>
	<li>        <span class="string">"update"</span> : <span class="number">1</span>,</li>
	<li>        <span class="string">"delete"</span> : <span class="number">0</span>,</li>
	<li>        <span class="string">"getmore"</span> : <span class="number">0</span>,</li>
	<li>        <span class="string">"command"</span> : <span class="number">75</span></li>
	<li>    },</li>
	<li>    <span class="string">"asserts"</span> : {</li>
	<li>        <span class="string">"regular"</span> : <span class="number">0</span>,</li>
	<li>        <span class="string">"warning"</span> : <span class="number">0</span>,</li>
	<li>        <span class="string">"msg"</span> : <span class="number">0</span>,</li>
	<li>        <span class="string">"user"</span> : <span class="number">1</span>,</li>
	<li>        <span class="string">"rollovers"</span> : <span class="number">0</span></li>
	<li>    },</li>
	<li>    <span class="string">"writeBacksQueued"</span> : false,</li>
	<li>    <span class="string">"ok"</span> : <span class="number">1</span></li>
	<li>}</li>
</ol>
</div>
&nbsp;
