---
layout: post
title: 让Json更懂中文(JSON_UNESCAPED_UNICODE)
date: 2014-12-10 15:57
author: admin
comments: true
categories: []
---
<div id="art_demo">我们知道, 用PHP的json_encode来处理中文的时候, 中文都会被编码, 变成不可读的, 类似”\u***”的格式, 还会在一定程度上增加传输的数据量.</div>
<div></div>
<div id="con_all">
<div id="con_da1"></div>
<div id="con_da8">
<div id="BAIDU_DUP_wrapper_u91397_0"></div>
</div>
</div>
<div id="art_content">
<div><a id="copybut42150"></a><span style="text-decoration: underline;">复制代码</span>代码如下:</div>
<div id="code42150">&lt;?php
echo json_encode("中文"); //"\u4e2d\u6587"</div>
这就让我们这些在天朝做开发的同学, 很是头疼, 有的时候还不得不自己写json_encode.

而在PHP5.4, 这个问题终于得以解决, Json新增了一个选项: JSON_UNESCAPED_UNICODE, 故名思议, 就是说, Json不要编码Unicode.

看下面的例子:
<div><a id="copybut33864"></a><span style="text-decoration: underline;">复制代码</span>代码如下:</div>
<div id="code33864">&lt;?php
echo json_encode("中文", JSON_UNESCAPED_UNICODE); //"中文"</div>
怎么样, 是不是让大家很开心的改动? 呵呵, 当然, Json在5.4还加入了: JSON_BIGINT_AS_STRING, JSON_PRETTY_PRINT, JSON_UNESCAPED_SLASHES等选项, 如果有兴趣, 大家可以参看: <a href="http://www.php.net/manual/en/function.json-encode.php" target="_blank">json_encode</a>

不过, 还是要提醒下: PHP 5.4还处于开发阶段, 在最终release之前, 任何新特性都可能被调整或者更改. 如果大家有任何建议, 也欢迎反馈, 帮助我们使得PHP变得更好.

</div>
