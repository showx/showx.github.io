---
layout: post
title: ubuntu 上安装 shadowsocks server
date: 2015-01-24 23:24
author: admin
comments: true
categories: []
---
<ol class="linenums">
	<li class="L0" value="1"><span class="pln">apt<span class="pun">-<span class="kwd">get<span class="pln"> install python<span class="pun">-<span class="pln">gevent python<span class="pun">-<span class="pln">pip</span></span></span></span></span></span></span></span></li>
</ol>
&nbsp;
<ol class="linenums">
	<li class="L0" value="1"><span class="pln">pip install shadowsocks</span></li>
</ol>
安装shadowsocks了。
接下来配置也比较简单，找到shadowsocks文件夹： sudo find / -name shadows*
新建一个 config.json，或者其他名字的都行，位置可以放在/etc/shadowsocks/下（默认没有这个文件，你要自己创建一个），或者home或者其他地方。

内容是
<ol class="linenums">
	<li class="L0" value="1"><span class="pun">{</span></li>
	<li class="L1"><span class="pln"><span class="str">"server"<span class="pun">:<span class="str">"my_server_ip"<span class="pun">,</span></span></span></span></span></li>
	<li class="L2"><span class="pln"><span class="str">"server_port"<span class="pun">:<span class="lit">8388<span class="pun">,</span></span></span></span></span></li>
	<li class="L3"><span class="pln"><span class="str">"local_port"<span class="pun">:<span class="lit">1080<span class="pun">,</span></span></span></span></span></li>
	<li class="L4"><span class="pln"><span class="str">"password"<span class="pun">:<span class="str">"barfoo!"<span class="pun">,</span></span></span></span></span></li>
	<li class="L5"><span class="pln"><span class="str">"timeout"<span class="pun">:<span class="lit">600<span class="pun">,</span></span></span></span></span></li>
	<li class="L6"><span class="pln"><span class="str">"method"<span class="pun">:<span class="str">"aes-256-cfb"</span></span></span></span></li>
	<li class="L7"><span class="pun">}</span></li>
</ol>
具体含义wiki上给的也很清楚
<ol class="linenums">
	<li class="L0" value="1"><span class="pln">server <span class="pun">服务器<span class="pln"> IP <span class="pun">(<span class="typ">IPv4<span class="pun">/<span class="typ">IPv6<span class="pun">)，注意这也将是服务端监听的<span class="pln"> IP <span class="pun">地址</span></span></span></span></span></span></span></span></span></span></li>
	<li class="L1"><span class="pln">server_port <span class="pun">服务器端口</span></span></li>
	<li class="L2"><span class="pln">local_port <span class="pun">本地端端口</span></span></li>
	<li class="L3"><span class="pln">password <span class="pun">用来加密的密码</span></span></li>
	<li class="L4"><span class="pln">timeout <span class="pun">超时时间（秒）</span></span></li>
	<li class="L5"><span class="pln">method <span class="pun">加密方法，可选择<span class="pln"> <span class="str">"bf-cfb"<span class="pun">,<span class="pln"> <span class="str">"aes-256-cfb"<span class="pun">,<span class="pln"> <span class="str">"des-cfb"<span class="pun">,<span class="pln"> <span class="str">"rc4"<span class="pun">,<span class="pln"> <span class="pun">等等。默认是一种不安全的加密，推荐用<span class="pln"> <span class="str">"aes-256-cfb"</span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></li>
</ol>
&nbsp;
<ol class="linenums">
	<li class="L0" value="1"><span class="pln">apt<span class="pun">-<span class="kwd">get<span class="pln"> install python<span class="pun">-<span class="pln">m2crypto</span></span></span></span></span></span></li>
</ol>
然后就可以启动服务了。
<ol class="linenums">
	<li class="L0" value="1"><span class="pln">ssserver <span class="pun">-<span class="pln">c <span class="pun">/<span class="pln">etc<span class="pun">/<span class="pln">shadowsocks<span class="pun">/<span class="pln">config<span class="pun">.<span class="pln">json</span></span></span></span></span></span></span></span></span></span></span></li>
</ol>
当然了，你不可能一直开着ssh，所以还是
<ol class="linenums">
	<li class="L0" value="1"><span class="pln">nohup ssserver <span class="pun">-<span class="pln">c <span class="pun">/<span class="pln">etc<span class="pun">/<span class="pln">shadowsocks<span class="pun">/<span class="pln">config<span class="pun">.<span class="pln">json <span class="pun">&amp;<span class="pln">gt<span class="pun">;<span class="pln"> log <span class="pun">&amp;<span class="pln">amp<span class="pun">;</span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></span></li>
</ol>
然后可以关了SSH。
或者更直接的开机自启动，添加到rc.local

&nbsp;

&nbsp;
