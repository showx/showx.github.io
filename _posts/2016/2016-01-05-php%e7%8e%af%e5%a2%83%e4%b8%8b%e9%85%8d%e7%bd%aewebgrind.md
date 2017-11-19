---
layout: post
title: PHP环境下配置WebGrind
date: 2016-01-05 10:49
author: admin
comments: true
categories: []
---
<strong>　　1、需要组件环境</strong>：

<strong>　　　　PHP 5.3</strong>

<strong>　　　　Apache服务器</strong>

<strong>　　　　xdebug</strong>

我自己用的是Wamp 2.1，不过用什么样的配置方法都是一样的，无非改改PHP.ini，在组件里添加文件。

闲话少说，正式开始“玩”这个所谓的<strong>WebGrind。</strong>

<strong>　　第一步：查看自己的版本中是否存在WebGrind；</strong>

<strong>　　　　一般wamp的首页有这个选项，当然你也可以通过访问 <a href="http://127.0.0.1/webgrin">http://127.0.0.1/webgrin</a>d 来查看是否存在；目录在wamp/apps</strong>

<strong>　　　　当然你也可以下载，自己配置：</strong>

<strong>　　　　Xdebug下载地址：xdebug <a href="http://www.xdebug.org/">http://www.xdebug.org</a></strong>

<strong>　　　　WebGrind下载地址：<a href="http://code.google.com/p/webgrind/">http://code.google.com/p/webgrind/</a></strong>

<strong>　　第二步，配置php.ini文件:
</strong>

<strong>　　　　找到PHP.ini 中的xdebug,编辑那里的选项，我把所有的off都开启成on了，这样就可以了；我自己又在网上找到别的教程，加上了几句，不知道何用：
</strong>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre><strong>; XDEBUG Extension

zend_extension = "H:/wamp/bin/php/php5.3.8/zend_ext/php_xdebug-2.1.2-5.3-vc9-x86_64.dll"

[xdebug]
;from Internet start
xdebug.auto_trace=on
xdebug.collect_params=on
xdebug.collect_return=on
xdebug.trace_output_dir="H:/wamp/tmp"
;end
xdebug.remote_enable = on
xdebug.profiler_enable = on
xdebug.profiler_enable_trigger = on
xdebug.profiler_output_name = cachegrind.out.%t.%p
xdebug.profiler_output_dir = "H:/wamp/tmp"</strong></pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="http://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
第三步，测试一下，打开本地的任意一个php文件，WebGrind都会自动监测的，然后打开 <a href="http://127.0.0.1/webgrin">http://127.0.0.1/webgrin</a>d 查看那里的结果

<img src="http://pic002.cnblogs.com/images/2012/279934/2012052122293617.jpg" alt="" width="800px" />

<strong> </strong>

&nbsp;
