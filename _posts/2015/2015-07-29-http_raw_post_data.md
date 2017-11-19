---
layout: post
title: $HTTP_RAW_POST_DATA
date: 2015-07-29 17:51
author: admin
comments: true
categories: []
---
&nbsp;

http://php.net/manual/zh/reserved.variables.httprawpostdata.php

这是手册里写的

总是产生变量包含有原始的 POST 数据。否则，此变量仅在碰到未识别 MIME 类型的数据时产生。不过，访问原始 POST 数据的更好方法是 php://input。$HTTP_RAW_POST_DATA 对于 enctype="multipart/form-data" 表单数据不可用。

问题：    $HTTP_RAW_POST_DATA  == $_POST  吗？

照手册所写 ，答案应该就为否。
假如不一样的话，他们的区别是什么呢？


我知道答案了，如下：

The RAW / uninterpreted HTTP POst information can be accessed with:
<strong> $GLOBALS['HTTP_RAW_POST_DATA']</strong>
This is useful in cases where the post Content-Type is not something PHP understands (such as text/xml).

也就是说,基本上$GLOBALS['HTTP_RAW_POST_DATA'] 和 $_POST是一样的。但是如果post过来的数据不是PHP能够识别的，你可以用 $GLOBALS['HTTP_RAW_POST_DATA']来接收，比如 text/xml 或者 soap 等等。

PHP默认识别的数据类型是application/x-www.form-urlencoded标准的数据类型

用Content-Type=text/xml 类型，提交一个xml文档内容给了php server,要怎么获得这个POST数据。

The RAW / uninterpreted HTTP POST information can be accessed with:   $GLOBALS['HTTP_RAW_POST_DATA'] This is useful in cases where the post Content-Type is not something PHP understands (such as text/xml).

由于PHP默认只识别application/x-www.form-urlencoded标准的数据类型，因此，对型如text/xml的内容无法解析为$_POST数组，故保留原型，交给$GLOBALS['HTTP_RAW_POST_DATA'] 来接收。

另外还有一项 php://input 也可以实现此这个功能

php://input 允许读取 POST 的原始数据。和 $HTTP_RAW_POST_DATA 比起来，它给内存带来的压力较小，并且不需要任何特殊的 php.ini 设置。php://input 不能用于 enctype="multipart/form-data"。


应用
<div>
a.htm
------------------
&lt;form   action="post.php"   method="post"&gt;
&lt;input   type="text"   name="user"&gt;
&lt;input   type="password"   name="password"&gt;
&lt;input   type="submit"&gt;
&lt;/form&gt;

post.php
----------------------------
&lt;?   echo   file_get_contents("php://input");   ?&gt;</div>
