---
layout: post
title: Ubuntu 上 rsync + Inotify 实现文件变动时实时同步备份
date: 2015-12-15 10:11
author: admin
comments: true
categories: []
---
<h1>Ubuntu 上 冗余备份方案 rsync  + Inotify 实现文件变动时实时同步备份</h1>
2台服务器、A和B、实现实时备份、首先安装好 rsync、inotify

A：生产环境使用

B：备份服务器
<h2>第一步、检查包是否安装</h2>
Ubuntu上用dpkg -l | grep 【包名】 检测  如下、我的都已安装、没安装的 apt-get 安装、Centos 就 yum
<div id="highlighter_264701" class="syntaxhighlighter  Brush">
<div class="lines">
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>1</code></td>
<td class="content"><code class="Brush plain">root@debian:~</code><code class="Brush comments"># dpkg -l | grep rsync</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>2</code></td>
<td class="content"><code class="Brush plain">ii </code><code class="Brush functions">rsync</code> <code class="Brush plain">3.0.9-4 amd64 fast, versatile, remote (and </code><code class="Brush functions">local</code><code class="Brush plain">) </code><code class="Brush functions">file</code><code class="Brush plain">-copying tool</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>3</code></td>
<td class="content"><code class="Brush plain">root@debian:~</code><code class="Brush comments"># dpkg -l | grep inotify</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>4</code></td>
<td class="content"><code class="Brush plain">ii inotify-tools 3.14-1 amd64 </code><code class="Brush functions">command</code><code class="Brush plain">-line programs providing a simple interface to inotify</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>5</code></td>
<td class="content"><code class="Brush plain">ii libinotifytools0 3.14-1 amd64 utility wrapper around inotify</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<h2>第二步、配置2台服务器</h2>
<h3>先配置B、备份服务器</h3>
<div id="highlighter_538158" class="syntaxhighlighter  Brush">
<div class="lines">
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>01</code></td>
<td class="content"><code class="Brush plain">port=873</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>02</code></td>
<td class="content"><code class="Brush plain">strict modes=</code><code class="Brush functions">yes</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>03</code></td>
<td class="content"><code class="Brush plain">log </code><code class="Brush functions">file</code><code class="Brush plain">=</code><code class="Brush plain">/var/log/rsyncd</code><code class="Brush plain">.log</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>04</code></td>
<td class="content"><code class="Brush plain">pid </code><code class="Brush functions">file</code><code class="Brush plain">=</code><code class="Brush plain">/usr/local/rsync/rsyncd</code><code class="Brush plain">.pid</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>05</code></td>
<td class="content"><code class="Brush plain">hosts allow =192.168.0.1</code><code class="Brush comments">#生产环境服务器IP、自定义</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>06</code></td>
<td class="content"><code class="Brush plain">ignore errors</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>07</code></td>
<td class="content"><code class="Brush plain">max connections = 5</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>08</code></td>
<td class="content"><code class="Brush plain">uid=root</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>09</code></td>
<td class="content"><code class="Brush plain">gid=root</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>10</code></td>
<td class="content"><code class="Brush functions">read</code> <code class="Brush plain">only =no</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>11</code></td>
<td class="content"><code class="Brush plain">write only =no</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>12</code></td>
<td class="content"><code class="Brush plain">auth </code><code class="Brush functions">users</code> <code class="Brush plain">= mysync </code><code class="Brush comments">#A服务器验证的用户名、自定义</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>13</code></td>
<td class="content"><code class="Brush plain">secrets </code><code class="Brush functions">file</code> <code class="Brush plain">=</code><code class="Brush plain">/usr/local/rsync/rsync</code><code class="Brush plain">.pass</code><code class="Brush comments">#A服务器验证的用户名、密码 、自定义</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>14</code></td>
<td class="content"><code class="Brush plain">[</code><code class="Brush functions">test</code><code class="Brush plain">]</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>15</code></td>
<td class="content"><code class="Brush plain">path =</code><code class="Brush plain">/home/www/test/</code> <code class="Brush comments">#建立备份项目路径</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
注意/usr/local/rsync/rsync.pass 必须是 600权限、不然会报错的
<pre>/usr/local/rsync/rsync.pass 内容

mysync:iamispass #多个就换行继续写、用户名密码记住、到时候A服务器配置时需要指定</pre>
<h3>再来配置A、生产服务器、源服务器</h3>
将B服务器的密码写在一个文件内、文件权限 600
<div id="highlighter_862739" class="syntaxhighlighter  Brush">
<div class="lines">
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>01</code></td>
<td class="content"><code class="Brush preprocessor bold">#!/bin/sh</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>02</code></td>
<td class="content"><code class="Brush comments">#同步脚本</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>03</code></td>
<td class="content"><code class="Brush plain">SRC=</code><code class="Brush plain">/home/myweb/test/</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>04</code></td>
<td class="content"><code class="Brush plain">DES=</code><code class="Brush functions">test</code>  <code class="Brush comments">#B服务器配置的项目名 [ ]内的那个名称，要对应上</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>05</code></td>
<td class="content"><code class="Brush plain">WEB=192.168.1.100 </code><code class="Brush comments">#备份服务器的IP</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>06</code></td>
<td class="content"><code class="Brush plain">USER=mysync  </code><code class="Brush comments">#用户名 B服务器的用户名</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>07</code></td>
<td class="content"><code class="Brush comments">#监控文件改变</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>08</code></td>
<td class="content"><code class="Brush plain">inotifywait -mrq --timefmt </code><code class="Brush string">'%d/%m/%y-%H:%M'</code> <code class="Brush plain">--</code><code class="Brush functions">format</code> <code class="Brush string">'%T %w%f%e'</code><code class="Brush plain">-e create,move,modify,attrib $SRC | </code><code class="Brush keyword">while</code> <code class="Brush functions">read</code> <code class="Brush plain">files</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>09</code></td>
<td class="content"><code class="Brush keyword">do</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>10</code></td>
<td class="content"><code class="Brush comments">#改变执行同步</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>11</code></td>
<td class="content"><code class="Brush functions">rsync</code><code class="Brush plain">-ahqzt --port=873 --password-</code><code class="Brush functions">file</code><code class="Brush plain">=</code><code class="Brush plain">/home/rsync/rsync</code><code class="Brush plain">.pass --delete $SRC $USER@$WEB::$DES</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>12</code></td>
<td class="content"><code class="Brush comments">#记录日志</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>13</code></td>
<td class="content"><code class="Brush functions">echo</code> <code class="Brush string">"${files} was rsynced"</code> <code class="Brush plain">&gt;&gt; </code><code class="Brush plain">/tmp/rsync</code><code class="Brush plain">.log 2&gt;&amp;1</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>14</code></td>
<td class="content"><code class="Brush keyword">done</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
执行脚本就好了

内容

iamispass #与A服务器写的密码对应上就行

在写一个脚本执行实时备份
<blockquote> RSYNC 参数

–exclude                                        ：指定避开同步的目录（只能写相对路径）

–exclude-from=/exclude.list         ：如果要避开同步的太多、可以写在一个文件里面（文件绝对路径）</blockquote>
