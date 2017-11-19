---
layout: post
title: PHP: 深入pack/unpack
date: 2015-07-15 08:46
author: admin
comments: true
categories: []
---
PHP作为一门为web而生的服务器端开发语言，被越来越多的公司所采用。其中不乏大公司，如腾迅、盛大、淘米、新浪等。在对性能要求比较高的项目中，PHP也逐渐演变成一门前端语言，用于访问后端接口。或者不同项目之间需要共享数据的时候，通常可以抽取出数据层，通过PHP来访问。
<h2>写在前面的话</h2>
本文介绍的是通过二进制数据包的方式通信，演示语言为PHP和Golang。PHP提供了pack/unpack函数来进行二进制打包和二进制解包。在具体讲解之前，我们先来了解一些基础知识。
<h2>什么是字节序</h2>
在不同的计算机体系结构中，对于数据(比特、字节、字)等的存储和传输机制有所不同，因而引发了计算机领域中一个潜在但是又很重要的问题，即通信双方交流的信息单元应该以什么样的顺序进行传送。如果达不成一致的规则，计算机的通信与存储将会无法进行。目前在各种体系的计算机中通常采用的字节存储机制主要有两种：大端(Big-endian)和小端(Little-endian)。这里所说的大端和小端即是字节序。
<h3>MSB和LSB</h3>
<ul>
	<li>MSB是Most Significant Bit/Byte的首字母缩写，通常译为最重要的位或最重要的字节。它通常用来表示在一个bit序列(如一个byte是8个bit组成的一个序列)或一个byte序列(如word是两个byte组成的一个序列)中对整个序列取值影响最大的那个bit/byte。</li>
	<li>LSB是Least Significant Bit/Byte的首字母缩写，通常译为最不重要的位或最不重要的字节。它通常用来表明在一个bit序列(如一个byte是8个bit组成的一个序列)或一个byte序列(如word是两个byte组成的一个序列)中对整个序列取值影响最小的那个bit/byte。</li>
	<li>对于一个十六进制int类型整数0x12345678来说，0x12就是MSB，0x78就是LSB。而对于0x78这个字节而言，它的二进制是01111000，那么最左边的那个0就是MSB，最右边的那个0就是LSB。</li>
</ul>
<h3>大端序</h3>
<ul>
	<li>大端序又叫网络字节序。大端序规定高位字节在存储时放在低地址上，在传输时高位字节放在流的开始；低位字节在存储时放在高地址上，在传输时低位字节放在流的末尾。</li>
</ul>
<h3>小端序</h3>
<ul>
	<li>小端序规定高位字节在存储时放在高地址上，在传输时高位字节放在流的末尾；低位字节在存储时放在低地址上，在传输时低位字节放在流的开始。</li>
</ul>
<h3>网络字节序</h3>
<ul>
	<li>网络字节序是指大端序。TCP/IP都是采用网络字节序的方式，java也是使用大端序方式存储。</li>
</ul>
<h3>主机字节序</h3>
<ul>
	<li>主机字节序代表本机的字节序。一般是小端序，但也有一些是大端序。</li>
	<li>主机字节序用在协议描述中则是指小端序。</li>
</ul>
<h3>总结</h3>
<ul>
	<li>字节序只针对于多字节类型的数据。比如对于int类型整数0x12345678，它占有4个字节的存储空间，存储方式有大端(0x12, 0x34, 0x56, 0x78)和小端(0x78, 0x56, 0x34, 0x12)两种。可以看到，在大端或小端的存储方式中，是以字节为单位的。所以对于单字节类型的数据，不存在字节序这个说法。</li>
</ul>
<h2>pack/unpack详解</h2>
PHP pack函数用于将其它进制的数字压缩到位字符串之中。也就是把其它进制数字转化为ASCII码字符串。

<a href="http://static.oschina.net/uploads/space/2014/0124/192402_Yzg7_182025.jpg" target="_blank"><img src="http://static.oschina.net/uploads/space/2014/0124/192402_Yzg7_182025.jpg" alt="" /></a>
<h3>格式字符翻译</h3>
<ul>
	<li>a -- 将字符串空白以 NULL 字符填满</li>
	<li>A -- 将字符串空白以 SPACE 字符 (空格) 填满</li>
	<li>h -- 16进制字符串，低位在前以半字节为单位</li>
	<li>H -- 16进制字符串，高位在前以半字节为单位</li>
	<li>c -- 有符号字符</li>
	<li>C -- 无符号字符</li>
	<li>s -- 有符号短整数 (16位，主机字节序)</li>
	<li>S -- 无符号短整数 (16位，主机字节序)</li>
	<li>n -- 无符号短整数 (16位, 大端字节序)</li>
	<li>v -- 无符号短整数 (16位, 小端字节序)</li>
	<li>i -- 有符号整数 (依赖机器大小及字节序)</li>
	<li>I -- 无符号整数 (依赖机器大小及字节序)</li>
	<li>l -- 有符号长整数 (32位，主机字节序)</li>
	<li>L -- 无符号长整数 (32位，主机字节序)</li>
	<li>N -- 无符号长整数 (32位, 大端字节序)</li>
	<li>V -- 无符号长整数 (32位, 小端字节序)</li>
	<li>f -- 单精度浮点数 (依计算机的范围)</li>
	<li>d -- 双精度浮点数 (依计算机的范围)</li>
	<li>x -- 空字节</li>
	<li>X -- 倒回一位</li>
	<li>@ -- 填入 NULL 字符到绝对位置</li>
</ul>
<h3>格式字符详解</h3>
<ul>
	<li>pack/unpack允许使用修饰符*和数字，紧跟在格式字符之后，用于指定该格式的个数；</li>
	<li>a和A都是用来打包字符串的，它们的唯一区别就是当小于定长时的填充方式。a以NULL填充，NULL事实上是'\0'的表示，代表空字节，8个位上全是0。A以空格填充，空格也即ASCII码为32的字符。这里有一个关于填充的使用场景的例子：请求登录的数据包规定用户名不超过20个字节，密码经过md5加密后是固定的32个字节。用户名就是变长的，为了便于服务器端读取和处理，通常会填充成定长。当然，这只是使用的方式之一，事实上还可以用变长的方式传递数据包，但这不在本文的探讨范围内。字符串有一点麻烦的是编码问题，尤其是在跟不同的平台通信时更为突出。比如在用pack进行打包字符串时，事实上是将字符内部的编码打包进去。单字节字符就没有问题，因为单字节在所有平台上都是一致的。来看个例子(pack.php)：</li>
</ul>
<div>
<div id="highlighter_239156" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$bin</code> <code class="php plain">= pack(</code><code class="php string">"a"</code><code class="php plain">, </code><code class="php string">"d"</code><code class="php plain">);</code></div>
<div class="line number3 index2 alt2"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. </code><code class="php variable">$bin</code> <code class="php plain">. </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
<div class="line number4 index3 alt1"><code class="php functions">echo</code> <code class="php string">"output: 0x"</code> <code class="php plain">. bin2hex(</code><code class="php variable">$bin</code><code class="php plain">) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_65358" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: d</code></div>
<div class="line number3 index2 alt2"><code class="shell plain">output: 0x64</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
$bin是返回的二进制字符，您可以直接输出它，PHP知道如何处理。通过bin2hex方法将$bin转换成十六进制可以知道，十六进制0x64表示的是字符d。对于中文字符(多字节字符)来说，通常有GBK编码、BIG5编码以及UTF8编码等。比如在GBK编码中，一个中文字符采用2个字节来表示；在UTF8编码中，一个中文字符采用3个字节来表示。这通常需要协商采用统一的编码，否则会由于内部的表示不一致导致无法处理。在PHP中只要将文件保存为特定的编码格即可，其它语言可能跟操作系统相关，因此或许需要编码转换。本文的例子一概基于UTF8编码。继续来看个例子：
<div>
<div id="highlighter_387494" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$bin</code> <code class="php plain">= pack(</code><code class="php string">"a3"</code><code class="php plain">, </code><code class="php string">"中"</code><code class="php plain">);</code></div>
<div class="line number3 index2 alt2"><code class="php functions">echo</code> <code class="php string">"output: 0x"</code> <code class="php plain">. bin2hex(</code><code class="php variable">$bin</code><code class="php plain">) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
<div class="line number4 index3 alt1"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. </code><code class="php functions">chr</code><code class="php plain">(0xe4) . </code><code class="php functions">chr</code><code class="php plain">(0xb8) . </code><code class="php functions">chr</code><code class="php plain">(0xad) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
<div class="line number5 index4 alt2"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. </code><code class="php variable">$bin</code><code class="php plain">{0} . </code><code class="php variable">$bin</code><code class="php plain">{1} . </code><code class="php variable">$bin</code><code class="php plain">{2} . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_312842" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: 0xe4b8ad</code></div>
<div class="line number3 index2 alt2"><code class="shell plain">output: 中</code></div>
<div class="line number4 index3 alt1"><code class="shell plain">output: 中</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
您可能会觉得很奇怪，后面2个输出是一样的。ASCII码表示单字节字符(其中包括英文字母、数字、英文标点符号、不可见字符以及控制字符等等)，它总是小于0x80，即小于十进制的128。当在处理字符时，如果字节小于0x80，则把它当作单字节来处理，否则会继续读取下一个字节，这通常跟编码有关，GBK会将2个字节当成一个字符来处理，UTF8则需要3个字节。有时候在PHP中需要做类似的处理，比如计算字符串中字符的个数(字符串可能包含单字节和多字节)，strlen方法只能计算字节数，而mb_strlen需要开启扩展。类似这样的需求，其实很容易处理：
<div>
<div id="highlighter_534160" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div>
<div class="line number18 index17 alt1">18</div>
<div class="line number19 index18 alt2">19</div>
<div class="line number20 index19 alt1">20</div>
<div class="line number21 index20 alt2">21</div>
<div class="line number22 index21 alt1">22</div>
<div class="line number23 index22 alt2">23</div>
<div class="line number24 index23 alt1">24</div>
<div class="line number25 index24 alt2">25</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php keyword">function</code> <code class="php plain">mbstrlen(</code><code class="php variable">$str</code><code class="php plain">)</code></div>
<div class="line number3 index2 alt2"><code class="php plain">{</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">    </code><code class="php variable">$len</code> <code class="php plain">= </code><code class="php functions">strlen</code><code class="php plain">(</code><code class="php variable">$str</code><code class="php plain">);</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">    </code></div>
<div class="line number6 index5 alt1"><code class="php spaces">    </code><code class="php keyword">if</code> <code class="php plain">(</code><code class="php variable">$len</code> <code class="php plain">&lt;= 0)</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">    </code><code class="php plain">{</code></div>
<div class="line number8 index7 alt1"><code class="php spaces">        </code><code class="php keyword">return</code> <code class="php plain">0;</code></div>
<div class="line number9 index8 alt2"><code class="php spaces">    </code><code class="php plain">}</code></div>
<div class="line number10 index9 alt1"><code class="php spaces">    </code></div>
<div class="line number11 index10 alt2"><code class="php spaces">    </code><code class="php variable">$count</code>  <code class="php plain">= 0;</code></div>
<div class="line number12 index11 alt1"><code class="php spaces">    </code></div>
<div class="line number13 index12 alt2"><code class="php spaces">    </code><code class="php keyword">for</code> <code class="php plain">(</code><code class="php variable">$i</code> <code class="php plain">= 0; </code><code class="php variable">$i</code> <code class="php plain">&lt; </code><code class="php variable">$len</code><code class="php plain">; </code><code class="php variable">$i</code><code class="php plain">++)</code></div>
<div class="line number14 index13 alt1"><code class="php spaces">    </code><code class="php plain">{</code></div>
<div class="line number15 index14 alt2"><code class="php spaces">        </code><code class="php variable">$count</code><code class="php plain">++;</code></div>
<div class="line number16 index15 alt1"><code class="php spaces">        </code><code class="php keyword">if</code> <code class="php plain">(ord(</code><code class="php variable">$str</code><code class="php plain">{</code><code class="php variable">$i</code><code class="php plain">}) &gt;= 0x80)</code></div>
<div class="line number17 index16 alt2"><code class="php spaces">        </code><code class="php plain">{</code></div>
<div class="line number18 index17 alt1"><code class="php spaces">            </code><code class="php variable">$i</code> <code class="php plain">+= 2;</code></div>
<div class="line number19 index18 alt2"><code class="php spaces">        </code><code class="php plain">}</code></div>
<div class="line number20 index19 alt1"><code class="php spaces">    </code><code class="php plain">}</code></div>
<div class="line number21 index20 alt2"><code class="php spaces">    </code></div>
<div class="line number22 index21 alt1"><code class="php spaces">    </code><code class="php keyword">return</code> <code class="php variable">$count</code><code class="php plain">;</code></div>
<div class="line number23 index22 alt2"><code class="php plain">}</code></div>
<div class="line number24 index23 alt1"></div>
<div class="line number25 index24 alt2"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. mbstrlen(</code><code class="php string">"中国so强大！"</code><code class="php plain">) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_795599" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: 7</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
以上代码的实现就是利用单字节字符的ASCII码小于0x80。至于要跳过几个字节，这要看具体是什么编码。接下来通过例子来看看a和A的区别：

$GOPATH/src

----pack_test

--------main.go

main.go的源码(只是用于测试，没有考虑细节)：
<div>
<div id="highlighter_693457" class="syntaxhighlighter  cpp">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div>
<div class="line number18 index17 alt1">18</div>
<div class="line number19 index18 alt2">19</div>
<div class="line number20 index19 alt1">20</div>
<div class="line number21 index20 alt2">21</div>
<div class="line number22 index21 alt1">22</div>
<div class="line number23 index22 alt2">23</div>
<div class="line number24 index23 alt1">24</div>
<div class="line number25 index24 alt2">25</div>
<div class="line number26 index25 alt1">26</div>
<div class="line number27 index26 alt2">27</div>
<div class="line number28 index27 alt1">28</div>
<div class="line number29 index28 alt2">29</div>
<div class="line number30 index29 alt1">30</div>
<div class="line number31 index30 alt2">31</div>
<div class="line number32 index31 alt1">32</div>
<div class="line number33 index32 alt2">33</div>
<div class="line number34 index33 alt1">34</div>
<div class="line number35 index34 alt2">35</div>
<div class="line number36 index35 alt1">36</div>
<div class="line number37 index36 alt2">37</div>
<div class="line number38 index37 alt1">38</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="cpp plain">package main</code></div>
<div class="line number2 index1 alt1"></div>
<div class="line number3 index2 alt2"><code class="cpp plain">import (</code></div>
<div class="line number4 index3 alt1"><code class="cpp spaces">    </code><code class="cpp string">"fmt"</code></div>
<div class="line number5 index4 alt2"><code class="cpp spaces">    </code><code class="cpp string">"net"</code></div>
<div class="line number6 index5 alt1"><code class="cpp plain">)</code></div>
<div class="line number7 index6 alt2"></div>
<div class="line number8 index7 alt1"><code class="cpp keyword bold">const</code> <code class="cpp plain">BUF_SIZE = 20</code></div>
<div class="line number9 index8 alt2"></div>
<div class="line number10 index9 alt1"><code class="cpp plain">func handleConnection(conn net.Conn) {</code></div>
<div class="line number11 index10 alt2"><code class="cpp spaces">    </code><code class="cpp plain">defer conn.Close()</code></div>
<div class="line number12 index11 alt1"><code class="cpp spaces">    </code><code class="cpp plain">buf := make([]byte, BUF_SIZE)</code></div>
<div class="line number13 index12 alt2"><code class="cpp spaces">    </code><code class="cpp plain">n, err := conn.Read(buf)</code></div>
<div class="line number14 index13 alt1"><code class="cpp spaces">    </code></div>
<div class="line number15 index14 alt2"><code class="cpp spaces">    </code><code class="cpp keyword bold">if</code> <code class="cpp plain">err != nil {</code></div>
<div class="line number16 index15 alt1"><code class="cpp spaces">        </code><code class="cpp plain">fmt.Printf(</code><code class="cpp string">"err: %v\n"</code><code class="cpp plain">, err)</code></div>
<div class="line number17 index16 alt2"><code class="cpp spaces">        </code><code class="cpp keyword bold">return</code></div>
<div class="line number18 index17 alt1"><code class="cpp spaces">    </code><code class="cpp plain">}</code></div>
<div class="line number19 index18 alt2"></div>
<div class="line number20 index19 alt1"><code class="cpp spaces">    </code><code class="cpp plain">fmt.Printf(</code><code class="cpp string">"\n已接收：%d个字节，数据是：'%s'\n"</code><code class="cpp plain">, n, string(buf))</code></div>
<div class="line number21 index20 alt2"><code class="cpp plain">}</code></div>
<div class="line number22 index21 alt1"></div>
<div class="line number23 index22 alt2"><code class="cpp plain">func main() {</code></div>
<div class="line number24 index23 alt1"><code class="cpp spaces">    </code><code class="cpp plain">ln, err := net.Listen(</code><code class="cpp string">"tcp"</code><code class="cpp plain">, </code><code class="cpp string">":9872"</code><code class="cpp plain">)</code></div>
<div class="line number25 index24 alt2"><code class="cpp spaces">    </code></div>
<div class="line number26 index25 alt1"><code class="cpp spaces">    </code><code class="cpp keyword bold">if</code> <code class="cpp plain">err != nil {</code></div>
<div class="line number27 index26 alt2"><code class="cpp spaces">        </code><code class="cpp plain">fmt.Printf(</code><code class="cpp string">"error: %v\n"</code><code class="cpp plain">, err)</code></div>
<div class="line number28 index27 alt1"><code class="cpp spaces">        </code><code class="cpp keyword bold">return</code></div>
<div class="line number29 index28 alt2"><code class="cpp spaces">    </code><code class="cpp plain">}</code></div>
<div class="line number30 index29 alt1"></div>
<div class="line number31 index30 alt2"><code class="cpp spaces">    </code><code class="cpp keyword bold">for</code> <code class="cpp plain">{</code></div>
<div class="line number32 index31 alt1"><code class="cpp spaces">        </code><code class="cpp plain">conn, err := ln.Accept()</code></div>
<div class="line number33 index32 alt2"><code class="cpp spaces">        </code><code class="cpp keyword bold">if</code> <code class="cpp plain">err != nil {</code></div>
<div class="line number34 index33 alt1"><code class="cpp spaces">            </code><code class="cpp keyword bold">continue</code></div>
<div class="line number35 index34 alt2"><code class="cpp spaces">        </code><code class="cpp plain">}</code></div>
<div class="line number36 index35 alt1"><code class="cpp spaces">        </code><code class="cpp plain">go handleConnection(conn)</code></div>
<div class="line number37 index36 alt2"><code class="cpp spaces">    </code><code class="cpp plain">}</code></div>
<div class="line number38 index37 alt1"><code class="cpp plain">}</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
代码很简单，收到数据，然后输出。

pack.php
<div>
<div id="highlighter_139106" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div>
<div class="line number18 index17 alt1">18</div>
<div class="line number19 index18 alt2">19</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$host</code> <code class="php plain">= </code><code class="php string">"127.0.0.1"</code><code class="php plain">;</code></div>
<div class="line number3 index2 alt2"><code class="php variable">$port</code> <code class="php plain">= </code><code class="php string">"9872"</code><code class="php plain">;</code></div>
<div class="line number4 index3 alt1"></div>
<div class="line number5 index4 alt2"><code class="php variable">$socket</code> <code class="php plain">= socket_create(AF_INET, SOCK_STREAM, SOL_TCP)</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">  </code><code class="php keyword">or</code> <code class="php keyword">die</code><code class="php plain">(</code><code class="php string">"Unable to create socket\n"</code><code class="php plain">);</code></div>
<div class="line number7 index6 alt2"></div>
<div class="line number8 index7 alt1"><code class="php plain">@socket_connect(</code><code class="php variable">$socket</code><code class="php plain">, </code><code class="php variable">$host</code><code class="php plain">, </code><code class="php variable">$port</code><code class="php plain">) </code><code class="php keyword">or</code> <code class="php keyword">die</code><code class="php plain">(</code><code class="php string">"Connect error.\n"</code><code class="php plain">);</code></div>
<div class="line number9 index8 alt2"></div>
<div class="line number10 index9 alt1"><code class="php keyword">if</code> <code class="php plain">(</code><code class="php variable">$err</code> <code class="php plain">= socket_last_error(</code><code class="php variable">$socket</code><code class="php plain">))</code></div>
<div class="line number11 index10 alt2"><code class="php plain">{</code></div>
<div class="line number12 index11 alt1"></div>
<div class="line number13 index12 alt2"><code class="php spaces">  </code><code class="php plain">socket_close(</code><code class="php variable">$socket</code><code class="php plain">);</code></div>
<div class="line number14 index13 alt1"><code class="php spaces">  </code><code class="php keyword">die</code><code class="php plain">(socket_strerror(</code><code class="php variable">$err</code><code class="php plain">) . </code><code class="php string">"\n"</code><code class="php plain">);</code></div>
<div class="line number15 index14 alt2"><code class="php plain">}</code></div>
<div class="line number16 index15 alt1"></div>
<div class="line number17 index16 alt2"><code class="php variable">$binarydata</code> <code class="php plain">= pack(</code><code class="php string">"a20"</code><code class="php plain">, </code><code class="php string">"中国强大"</code><code class="php plain">);</code></div>
<div class="line number18 index17 alt1"><code class="php variable">$len</code> <code class="php plain">= socket_write (</code><code class="php variable">$socket</code> <code class="php plain">, </code><code class="php variable">$binarydata</code><code class="php plain">, </code><code class="php functions">strlen</code><code class="php plain">(</code><code class="php variable">$binarydata</code><code class="php plain">));</code></div>
<div class="line number19 index18 alt2"><code class="php plain">socket_close(</code><code class="php variable">$socket</code><code class="php plain">);</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_147939" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ </code><code class="shell functions">cd</code> <code class="shell plain">$GOPATH</code><code class="shell plain">/src/pack_test</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">$ go build</code></div>
<div class="line number3 index2 alt2"><code class="shell plain">$ .</code><code class="shell plain">/pack_test</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_287575" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
当执行php后，可以看到服务器端在控制台输出：
<div>
<div id="highlighter_876216" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">已接收：20个字节，数据是：</code><code class="shell string">'中国强大'</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
以上的输出中，单引号不是数据的一部分，只是为了便于观察。很明显，我们打包的字符串只占12字节，a20表示20个a，您当然可以连续写20个a，但我想您不会这么傻。如果是a*的话，则表示任意多个a。通过服务器端的输出来看，PHP发送了20个字节过去，服务器端也接收了20个字节，但因为填充的\0是空字符，所以您不会看到有什么不一样的地方。现在我们将a20换成A20，代码如下：
<div>
<div id="highlighter_63264" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div>
<div class="line number18 index17 alt1">18</div>
<div class="line number19 index18 alt2">19</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$host</code> <code class="php plain">= </code><code class="php string">"127.0.0.1"</code><code class="php plain">;</code></div>
<div class="line number3 index2 alt2"><code class="php variable">$port</code> <code class="php plain">= </code><code class="php string">"9872"</code><code class="php plain">;</code></div>
<div class="line number4 index3 alt1"></div>
<div class="line number5 index4 alt2"><code class="php variable">$socket</code> <code class="php plain">= socket_create(AF_INET, SOCK_STREAM, SOL_TCP)</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">  </code><code class="php keyword">or</code> <code class="php keyword">die</code><code class="php plain">(</code><code class="php string">"Unable to create socket\n"</code><code class="php plain">);</code></div>
<div class="line number7 index6 alt2"></div>
<div class="line number8 index7 alt1"><code class="php plain">@socket_connect(</code><code class="php variable">$socket</code><code class="php plain">, </code><code class="php variable">$host</code><code class="php plain">, </code><code class="php variable">$port</code><code class="php plain">) </code><code class="php keyword">or</code> <code class="php keyword">die</code><code class="php plain">(</code><code class="php string">"Connect error.\n"</code><code class="php plain">);</code></div>
<div class="line number9 index8 alt2"></div>
<div class="line number10 index9 alt1"><code class="php keyword">if</code> <code class="php plain">(</code><code class="php variable">$err</code> <code class="php plain">= socket_last_error(</code><code class="php variable">$socket</code><code class="php plain">))</code></div>
<div class="line number11 index10 alt2"><code class="php plain">{</code></div>
<div class="line number12 index11 alt1"></div>
<div class="line number13 index12 alt2"><code class="php spaces">  </code><code class="php plain">socket_close(</code><code class="php variable">$socket</code><code class="php plain">);</code></div>
<div class="line number14 index13 alt1"><code class="php spaces">  </code><code class="php keyword">die</code><code class="php plain">(socket_strerror(</code><code class="php variable">$err</code><code class="php plain">) . </code><code class="php string">"\n"</code><code class="php plain">);</code></div>
<div class="line number15 index14 alt2"><code class="php plain">}</code></div>
<div class="line number16 index15 alt1"></div>
<div class="line number17 index16 alt2"><code class="php variable">$binarydata</code> <code class="php plain">= pack(</code><code class="php string">"A20"</code><code class="php plain">, </code><code class="php string">"中国强大"</code><code class="php plain">);</code></div>
<div class="line number18 index17 alt1"><code class="php variable">$len</code> <code class="php plain">= socket_write (</code><code class="php variable">$socket</code> <code class="php plain">, </code><code class="php variable">$binarydata</code><code class="php plain">, </code><code class="php functions">strlen</code><code class="php plain">(</code><code class="php variable">$binarydata</code><code class="php plain">));</code></div>
<div class="line number19 index18 alt2"><code class="php plain">socket_close(</code><code class="php variable">$socket</code><code class="php plain">);</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_871860" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">$ php -f pack.php</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
您会发现服务器端的输出不一样了：
<div>
<div id="highlighter_593516" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">已接收：20个字节，数据是：</code><code class="shell string">'中国强大        '</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
是的，空格存在于数据中。这就是a和A的区别。
<ul>
	<li>h和H的描述看起来有些奇怪。它们都是读取十进制，以十六进制方式读取，以半字节(4位)为单位。这听起来有些拗口，还是以实例来说明：</li>
</ul>
<div>
<div id="highlighter_150507" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. pack(</code><code class="php string">"H"</code><code class="php plain">, 0x5) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_632067" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: P</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
首先是读取十进制，所以0x5会转成十进制的5，然后以半字节为单位并且以十六进制方式读取，为了补足8位，所以需要在5后面补0，变成0x50。别忘了十六进制的一位相当于二进制的四位。0x50正好是字符P的ASCII码。
<div>
<div id="highlighter_170095" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. </code><code class="php functions">chr</code><code class="php plain">(0x50) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_891574" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: P</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
h和H的差别在于h是低位在前，H是高位在前，拿前面的例子来看看h的行为：
<div>
<div id="highlighter_72732" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$bin</code> <code class="php plain">= pack(</code><code class="php string">"h"</code><code class="php plain">, 0x5);</code></div>
<div class="line number3 index2 alt2"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. </code><code class="php variable">$bin</code> <code class="php plain">. </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
<div class="line number4 index3 alt1"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. ord(</code><code class="php variable">$bin</code><code class="php plain">) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_447993" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: </code></div>
<div class="line number3 index2 alt2"><code class="shell plain">output: 5</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
读取十进制的5，后面补0，变成十六进制的0x50，因为H是高位在前，所以没有变化，而h就需要将0x50变成0x05。由于0x05是不可见字符，所以上面的字符输出是空的。

h和H是以半字节为单位，h2和H2则表示一次读取8位，同理h3和H3可以推导出来，但是别忘了补足8位哦！
<div>
<div id="highlighter_149679" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. pack(</code><code class="php string">"H"</code><code class="php plain">, 0x47) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_735767" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: p</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
以上的代码中，0x47为十进制的71，因为读取半个字节，所以变成0x7，后面补0变成0x70，则刚好是字符p的ASCII码。如果换成是h格式化，则最终的结果是0x07，因为低位在前。

对于一次读取多个字节，也以同样的规则：
<div>
<div id="highlighter_857977" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. pack(</code><code class="php string">"H2h2"</code><code class="php plain">, 0x47, 0x56) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_422995" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: qh</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
0x47是十进制的71，由于使用H2格式化，所以一次读取8位，最后变成十六进制的0x71，即字符q的ASCII码。0x56是十进制的86，由于使用h2格式化，所以一次读取8位，最后变成十六进制的0x86，但是由于h表示低位在前，因此0x86变成0x68，即字符h的ASCII码。
<ul>
	<li>c和C都表示字符，前者表示有符号字符，后者表示无符号字符。</li>
</ul>
<div>
<div id="highlighter_725767" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. pack(</code><code class="php string">"c"</code><code class="php plain">, 65) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
<div class="line number3 index2 alt2"><code class="php functions">echo</code> <code class="php string">"output: "</code> <code class="php plain">. pack(</code><code class="php string">"C"</code><code class="php plain">, 65) . </code><code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_457094" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">output: A</code></div>
<div class="line number3 index2 alt2"><code class="shell plain">output: A</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<ul>
	<li>s为有符号短整数；S为无符号短整数。它们都为主机字节序，并且为16位。通常为主机字节序的格式化字符，一般只用于单机的操作，因为您无法确定主机字节序究竟是大端还是小端。当然，您一定要这么干的话，也是有办法来获取本机字节序是属于大端或小端，但那样是没有必要的。稍后就会给出一个通过PHP来判断字节序的例子。</li>
</ul>
<div>
<div id="highlighter_573574" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$bin1</code> <code class="php plain">= pack(</code><code class="php string">"s"</code><code class="php plain">, 345);</code></div>
<div class="line number3 index2 alt2"><code class="php variable">$bin2</code> <code class="php plain">= pack(</code><code class="php string">"S"</code><code class="php plain">, 452);</code></div>
<div class="line number4 index3 alt1"><code class="php plain">print_r(unpack(</code><code class="php string">"sshort1"</code><code class="php plain">, </code><code class="php variable">$bin1</code><code class="php plain">));</code></div>
<div class="line number5 index4 alt2"><code class="php plain">print_r(unpack(</code><code class="php string">"sshort2"</code><code class="php plain">, </code><code class="php variable">$bin2</code><code class="php plain">));</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_459838" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number9 index8 alt2">9</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">Array</code></div>
<div class="line number3 index2 alt2"><code class="shell plain">(</code></div>
<div class="line number4 index3 alt1"><code class="shell spaces">    </code><code class="shell plain">[short1] =&gt; 345</code></div>
<div class="line number5 index4 alt2"><code class="shell plain">)</code></div>
<div class="line number6 index5 alt1"><code class="shell plain">Array</code></div>
<div class="line number7 index6 alt2"><code class="shell plain">(</code></div>
<div class="line number8 index7 alt1"><code class="shell spaces">    </code><code class="shell plain">[short2] =&gt; 452</code></div>
<div class="line number9 index8 alt2"><code class="shell plain">)</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<ul>
	<li>n和v除了明确指定了字节序，其它行为跟s和S是一样的。</li>
	<li>i和I依赖于机器大小及字节序，很少用它们。</li>
	<li>l、L、N、V跟s、S、n、v类似，除了表示的大小不同，前者都为32位，后者都为16位。</li>
	<li>f、d是因为float和double与CPU无关。一般来说，编译器是按照IEEE标准解释的，即把float/double看作4/8个字符的数组进行解释。因此，只要编译器是支持IEEE浮点标准的，就不需要考虑字节顺序。</li>
	<li>剩下的x、X和@用得比较少，对此不作深究。</li>
</ul>
<h3>unpack的用法</h3>
<ul>
	<li>unpack是用来解包经过pack打包的数据包，如果成功，则返回数组。其中格式化字符和执行pack时一一对应，但是需要额外的指定一个key，用作返回数组的key。多个字段用/分隔。例如：</li>
</ul>
<div>
<div id="highlighter_777465" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number8 index7 alt1">8</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$bin</code> <code class="php plain">= @pack(</code><code class="php string">"a9SS"</code><code class="php plain">, </code><code class="php string">"陈一回"</code><code class="php plain">, 20, 1);</code></div>
<div class="line number3 index2 alt2"><code class="php variable">$data</code> <code class="php plain">= @unpack(</code><code class="php string">"a9name/sage/Sgender"</code><code class="php plain">, </code><code class="php variable">$bin</code><code class="php plain">);</code></div>
<div class="line number4 index3 alt1"></div>
<div class="line number5 index4 alt2"><code class="php keyword">if</code> <code class="php plain">(</code><code class="php functions">is_array</code><code class="php plain">(</code><code class="php variable">$data</code><code class="php plain">))</code></div>
<div class="line number6 index5 alt1"><code class="php plain">{</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">    </code><code class="php plain">print_r(</code><code class="php variable">$data</code><code class="php plain">);</code></div>
<div class="line number8 index7 alt1"><code class="php plain">}</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_560127" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number7 index6 alt2">7</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php  -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">Array</code></div>
<div class="line number3 index2 alt2"><code class="shell plain">(</code></div>
<div class="line number4 index3 alt1"><code class="shell spaces">    </code><code class="shell plain">[name] =&gt; 陈一回</code></div>
<div class="line number5 index4 alt2"><code class="shell spaces">    </code><code class="shell plain">[age] =&gt; 20</code></div>
<div class="line number6 index5 alt1"><code class="shell spaces">    </code><code class="shell plain">[gender] =&gt; 1</code></div>
<div class="line number7 index6 alt2"><code class="shell plain">)</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<h3>一些例子</h3>
<ul>
	<li>判断大小端</li>
</ul>
<div>
<div id="highlighter_170798" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div>
<div class="line number18 index17 alt1">18</div>
<div class="line number19 index18 alt2">19</div>
<div class="line number20 index19 alt1">20</div>
<div class="line number21 index20 alt2">21</div>
<div class="line number22 index21 alt1">22</div>
<div class="line number23 index22 alt2">23</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php keyword">function</code> <code class="php plain">IsBigEndian()</code></div>
<div class="line number3 index2 alt2"><code class="php plain">{</code></div>
<div class="line number4 index3 alt1"><code class="php spaces">    </code><code class="php variable">$bin</code> <code class="php plain">= pack(</code><code class="php string">"L"</code><code class="php plain">, 0x12345678);</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">    </code><code class="php variable">$hex</code> <code class="php plain">= bin2hex(</code><code class="php variable">$bin</code><code class="php plain">);</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">    </code><code class="php keyword">if</code> <code class="php plain">(ord(pack(</code><code class="php string">"H2"</code><code class="php plain">, </code><code class="php variable">$hex</code><code class="php plain">)) === 0x78)</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">    </code><code class="php plain">{</code></div>
<div class="line number8 index7 alt1"><code class="php spaces">        </code><code class="php keyword">return</code> <code class="php plain">FALSE;</code></div>
<div class="line number9 index8 alt2"><code class="php spaces">    </code><code class="php plain">}</code></div>
<div class="line number10 index9 alt1"></div>
<div class="line number11 index10 alt2"><code class="php spaces">    </code><code class="php keyword">return</code> <code class="php plain">TRUE;</code></div>
<div class="line number12 index11 alt1"><code class="php plain">}</code></div>
<div class="line number13 index12 alt2"></div>
<div class="line number14 index13 alt1"><code class="php keyword">if</code> <code class="php plain">(IsBigEndian())</code></div>
<div class="line number15 index14 alt2"><code class="php plain">{</code></div>
<div class="line number16 index15 alt1"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">"大端序"</code><code class="php plain">;</code></div>
<div class="line number17 index16 alt2"><code class="php plain">}</code></div>
<div class="line number18 index17 alt1"><code class="php keyword">else</code></div>
<div class="line number19 index18 alt2"><code class="php plain">{</code></div>
<div class="line number20 index19 alt1"><code class="php spaces">    </code><code class="php functions">echo</code> <code class="php string">"小端序"</code><code class="php plain">;</code></div>
<div class="line number21 index20 alt2"><code class="php plain">}</code></div>
<div class="line number22 index21 alt1"></div>
<div class="line number23 index22 alt2"><code class="php functions">echo</code> <code class="php string">"\n"</code><code class="php plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<div>
<div id="highlighter_527124" class="syntaxhighlighter  shell">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="shell plain">$ php -f pack.php</code></div>
<div class="line number2 index1 alt1"><code class="shell plain">小端序</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<ul>
	<li>网络通信

比如现在要通过PHP发送数据包到服务器来登录。在仅需要提供用户名(最多30个字节)和密码(md5之后固定为32字节)的情况下，可以构造如下数据包(当然这事先需要跟服务器协商好数据包的规范，本例以网络字节序通信)：

包结构：</li>
</ul>
<table>
<tbody>
<tr>
<td>字段</td>
<td>字节数</td>
<td>说明</td>
</tr>
<tr>
<td>包头</td>
<td>定长</td>
<td>每一个通信消息必须包含的内容</td>
</tr>
<tr>
<td>包体</td>
<td>不定长</td>
<td>根据每个通信消息的不同产生变化</td>
</tr>
</tbody>
</table>
其中包头详细内容如下：
<table>
<tbody>
<tr>
<td>字段</td>
<td>字节数</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr>
<td>pkg_len</td>
<td>2</td>
<td>ushort</td>
<td>整个包的长度，不超过4K</td>
</tr>
<tr>
<td>version</td>
<td>1</td>
<td>uchar</td>
<td>通讯协议版本号</td>
</tr>
<tr>
<td>command_id</td>
<td>2</td>
<td>ushort</td>
<td>消息命令ID</td>
</tr>
<tr>
<td>result</td>
<td>2</td>
<td>short</td>
<td>请求时不起作用；请求返回时使用</td>
</tr>
</tbody>
</table>
当然实际中可能会涉及到各种校验。本文为了简单，只是列举一下通常的工作流程及处理的方式。

登录(执行命储1001)
<table>
<tbody>
<tr>
<td>字段</td>
<td>字节数</td>
<td>类型</td>
<td>说明</td>
</tr>
<tr>
<td>用户名</td>
<td>30</td>
<td>uchar[30]</td>
<td>登录用户名</td>
</tr>
<tr>
<td>密码</td>
<td>32</td>
<td>uchar[32]</td>
<td>登录密码</td>
</tr>
</tbody>
</table>
包头是定长的，通过计算可知包头占7个字节，并且包头在包体之前。比如用户陈一回需要登录，密码是123456，则代码如下：
<div>
<div id="highlighter_928055" class="syntaxhighlighter  php">
<div class="toolbar"><a class="toolbar_item command_help help" href="http://my.oschina.net/goal/blog/195749#">?</a></div>
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
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$version</code>    <code class="php plain">= 1;</code></div>
<div class="line number3 index2 alt2"><code class="php variable">$result</code>     <code class="php plain">= 0;</code></div>
<div class="line number4 index3 alt1"><code class="php variable">$command_id</code> <code class="php plain">= 1001;</code></div>
<div class="line number5 index4 alt2"><code class="php variable">$username</code>   <code class="php plain">= </code><code class="php string">"陈一回"</code><code class="php plain">;</code></div>
<div class="line number6 index5 alt1"><code class="php variable">$password</code>   <code class="php plain">= md5(</code><code class="php string">"123456"</code><code class="php plain">);</code></div>
<div class="line number7 index6 alt2"><code class="php comments">// 构造包体</code></div>
<div class="line number8 index7 alt1"><code class="php variable">$bin_body</code>   <code class="php plain">= pack(</code><code class="php string">"a30a32"</code><code class="php plain">, </code><code class="php variable">$username</code><code class="php plain">, </code><code class="php variable">$password</code><code class="php plain">);</code></div>
<div class="line number9 index8 alt2"><code class="php comments">// 包体长度</code></div>
<div class="line number10 index9 alt1"><code class="php variable">$body_len</code>   <code class="php plain">= </code><code class="php functions">strlen</code><code class="php plain">(</code><code class="php variable">$bin_body</code><code class="php plain">);</code></div>
<div class="line number11 index10 alt2"><code class="php variable">$bin_head</code>   <code class="php plain">= pack(</code><code class="php string">"nCns"</code><code class="php plain">, </code><code class="php variable">$body_len</code><code class="php plain">, </code><code class="php variable">$version</code><code class="php plain">, </code><code class="php variable">$command_id</code><code class="php plain">, </code><code class="php variable">$result</code><code class="php plain">);</code></div>
<div class="line number12 index11 alt1"><code class="php variable">$bin_data</code>   <code class="php plain">= </code><code class="php variable">$bin_head</code> <code class="php plain">. </code><code class="php variable">$bin_body</code><code class="php plain">;</code></div>
<div class="line number13 index12 alt2"><code class="php comments">// 发送数据</code></div>
<div class="line number14 index13 alt1"><code class="php comments">// socket_write($socket, $bin_data, strlen($bin_data));</code></div>
<div class="line number15 index14 alt2"><code class="php comments">// socket_close($socket);</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
服务器端通过读取定长包头，拿到包体长度，再读取并解析包体。大致的过程就是这样。当然服务器端也会返回响应包，客户端做相应的读取处理。
