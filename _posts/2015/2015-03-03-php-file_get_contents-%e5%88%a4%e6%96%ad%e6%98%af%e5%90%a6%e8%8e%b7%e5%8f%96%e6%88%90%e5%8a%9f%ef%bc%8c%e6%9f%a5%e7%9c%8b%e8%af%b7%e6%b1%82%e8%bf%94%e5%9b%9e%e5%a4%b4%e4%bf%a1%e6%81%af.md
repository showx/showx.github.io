---
layout: post
title: PHP file_get_contents 判断是否获取成功，查看请求返回头信息
date: 2015-03-03 16:46
author: admin
comments: true
categories: []
---
PHP 简单快速的获取文件信息，可以用函数 file_get_contents（），包括网络文件信息，当然file_get_contents（）也有许多不稳定的因素，先来讲获取请求返回头信息 ；

<strong>示例：
</strong>
<div id="blog_content" class="blog_content">
<div class="dp-highlighter bg_php">
<div class="bar">
<div class="tools"><b>[php]</b> <a class="ViewSource" title="view plain" href="http://blog.csdn.net/yanlaizhishi/article/details/8566435#">view plain</a><a class="CopyToClipboard" title="copy" href="http://blog.csdn.net/yanlaizhishi/article/details/8566435#">copy</a>
<div><embed id="ZeroClipboardMovie_1" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_1"></embed></div>
</div>
</div>
<ol class="dp-c" start="1">
	<li class="alt">&lt;?php</li>
	<li class=""></li>
	<li class="alt">   <span class="comment">//加上@ 是为了防止file_get_contents获取失败返回至命错误，影响后面的程序运行</span></li>
	<li class=""></li>
	<li class="alt">   @<span class="func">file_get_contents</span>(<span class="string">"http://tqybw.net"</span>);</li>
	<li class="">   var_dump(<span class="vars">$http_response_header</span>);</li>
	<li class="alt">   <span class="comment">//&lt;var class="varname"&gt;&lt;var class="varname"&gt;$http_response_header&lt;/var&gt;&lt;/var&gt; &lt;span class="type"&gt;&lt;span class="type 数组"&gt;数组&lt;/span&gt;&lt;/span&gt;与 &lt;span class="function"&gt;get_headers()&lt;/span&gt; 函数类似。当使用HTTP 包装器时，&lt;var class="varname"&gt;&lt;var class="varname"&gt;$http_response_header&lt;/var&gt;&lt;/var&gt; 将会被 HTTP 响应头信息填充。&lt;var class="varname"&gt;&lt;var class="varname"&gt;    $http_response_header&lt;/var&gt;&lt;/var&gt; 将被创建于局部作用域中。  </span></li>
	<li class=""></li>
	<li class="alt">?&gt;</li>
	<li class=""></li>
	<li class="alt"><span class="comment">//输出结果</span></li>
	<li class=""></li>
	<li class="alt">  <span class="keyword">array</span>(9) {</li>
	<li class="">     [0]=&gt;</li>
	<li class="alt">     string(15) <span class="string">"HTTP/1.1 200 OK"</span>  <span class="comment">//这个很关键，通过返回状态码可以得知是否成功</span></li>
	<li class="">      [1]=&gt;</li>
	<li class="alt">     string(35) <span class="string">"Date: Sat, 12 Apr 2012 17:30:38 GMT"</span></li>
	<li class="">     [2]=&gt;</li>
	<li class="alt">     string(29) <span class="string">"Server: Apache/2.2.3 (CentOS)"</span></li>
	<li class="">     [3]=&gt;</li>
	<li class="alt">     string(44) <span class="string">"Last-Modified: Tue, 15 Nov 2005 13:24:10 GMT"</span></li>
	<li class="">     [4]=&gt;</li>
	<li class="alt">     string(27) <span class="string">"ETag: "</span>280100-1b6-80bfd280<span class="string">""</span></li>
	<li class="">     [5]=&gt;</li>
	<li class="alt">     string(20) <span class="string">"Accept-Ranges: bytes"</span></li>
	<li class="">     [6]=&gt;</li>
	<li class="alt">     string(19) <span class="string">"Content-Length: 438"</span></li>
	<li class="">     [7]=&gt;</li>
	<li class="alt">     string(17) <span class="string">"Connection: close"</span></li>
	<li class="">     [8]=&gt;</li>
	<li class="alt">     string(38) <span class="string">"Content-Type: text/html; charset=UTF-8"</span></li>
	<li class="">  }</li>
</ol>
</div>
<strong>注意：
</strong>
1、file_get_contents 用于获取网络文件时，如：（file_get_contents("http://tqybw.net/beijing15tian/")），不能频繁使用，特别是网络不稳定时，容易造成超时引起服务占资源

2、file_get_contents 获取文件大小有限制</div>
