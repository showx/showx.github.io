---
layout: post
title: php连接不上mysql但mysql命令行操作正常的解决方法
date: 2015-01-22 15:18
author: admin
comments: true
categories: []
---
故障状况：php网站连接mysql失败，但在命令行下通过mysql命令可登录并正常操作。
解决方案：
1、命令行下登录mysql，执行以下命令：
<div class="codetitle"><a id="copybut44211" class="copybut"></a><span style="text-decoration: underline;">复制代码</span>代码如下:</div>
<div id="code44211" class="codebody">show variables like 'socket';</div>
执行后会得到类似于如下回显：
<div class="codetitle"><a id="copybut35926" class="copybut"></a><span style="text-decoration: underline;">复制代码</span>代码如下:</div>
<div id="code35926" class="codebody">
"Variable_name"        "Value"
"socket"                  "/home/mysql/data/mysql.sock"</div>
2、编辑php.ini，找到mysql.default_socket配置项，默认一般是空值（使用编辑Mysql时设置的sock路径），将此项添加值为上面回显中的"/home/mysql/data/mysql.sock"：
<div class="codetitle"><a id="copybut13899" class="copybut"></a><span style="text-decoration: underline;">复制代码</span>代码如下:</div>
<div id="code13899" class="codebody">
; Default socket name for local MySQL connects.  If empty, uses the built-in
; MySQL defaults.
mysql.default_socket = /home/mysql/data/mysql.sock</div>
3、重启php。
