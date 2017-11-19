---
layout: post
title: Monolog：PHP 日志记录工具
date: 2015-07-28 16:05
author: admin
comments: true
categories: []
---
Monolog是php下比较全又容易扩展的记录日志组件。目前有包括Symfony 、Laravel、 CakePHP等诸多知名php框架都内置了Monolog。

Monolog可以把你的日志发送到文件，sockets，收件箱，数据库和各种web服务器上。一些特殊的组件可以给你带来特殊的日志策略。

<strong>使用例子</strong>
<div>
<div id="highlighter_571423" class="syntaxhighlighter notranslate php">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"></div>
<div class="line number3 index2 alt2"><code class="php keyword">use</code> <code class="php plain">Monolog\Logger;</code></div>
<div class="line number4 index3 alt1"><code class="php keyword">use</code> <code class="php plain">Monolog\Handler\StreamHandler;</code></div>
<div class="line number5 index4 alt2"></div>
<div class="line number6 index5 alt1"><code class="php comments">// create a log channel</code></div>
<div class="line number7 index6 alt2"><code class="php variable">$log</code> <code class="php plain">= </code><code class="php keyword">new</code> <code class="php plain">Logger(</code><code class="php string">'name'</code><code class="php plain">);</code></div>
<div class="line number8 index7 alt1"><code class="php variable">$log</code><code class="php plain">-&gt;pushHandler(</code><code class="php keyword">new</code> <code class="php plain">StreamHandler(</code><code class="php string">'path/to/your.log'</code><code class="php plain">, Logger::WARNING));</code></div>
<div class="line number9 index8 alt2"></div>
<div class="line number10 index9 alt1"><code class="php comments">// add records to the log</code></div>
<div class="line number11 index10 alt2"><code class="php variable">$log</code><code class="php plain">-&gt;addWarning(</code><code class="php string">'Foo'</code><code class="php plain">);</code></div>
<div class="line number12 index11 alt1"><code class="php variable">$log</code><code class="php plain">-&gt;addError(</code><code class="php string">'Bar'</code><code class="php plain">);</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<strong>核心概念</strong>

每个Logger实例都有一个通道和日志处理器栈。每当你添加一条日志记录，它会被发送到日志处理器栈。 你可以创建很多<code>Logger，</code>每个Logger定义一个通道（db，请求，路由），每个Logger有很多日志处理器。这些通道会过滤日志。

每个日志处理器都有一个Formatter（内置的日志显示格式处理器）。你还可以设定日志级别。

<strong>日志级别</strong>
<ol>
	<li> DEBUG：详细的debug信息</li>
	<li>INFO：感兴趣的事件。像用户登录，SQL日志</li>
	<li>NOTICE：正常但有重大意义的事件。</li>
	<li>WARNING<strong>：</strong>发生异常，使用了已经过时的API。</li>
	<li>ERROR：运行时发生了错误，错误需要记录下来并监视，但错误不需要立即处理。</li>
	<li>CRITICAL：关键错误，像应用中的组件不可用。</li>
	<li>ALETR：需要立即采取措施的错误，像整个网站挂掉了，数据库不可用。这个时候触发器会通过SMS通知你，</li>
</ol>
&nbsp;

&nbsp;
<p class="post_mem_info">官方网站：<a href="http://monolog.ow2.org/" target="_blank" rel="nofollow">http://monolog.ow2.org/</a>
开源地址：<a href="https://github.com/Seldaek/monolog" target="_blank" rel="nofollow">https://github.com/Seldaek/monolog</a></p>
