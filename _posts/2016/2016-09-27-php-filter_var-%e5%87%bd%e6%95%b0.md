---
layout: post
title: PHP filter_var() 函数
date: 2016-09-27 02:53
author: admin
comments: true
categories: [Uncategorized]
---
<em>一直以来，都是用正则表达式来进行email的格式验证，却不知道PHP本身有内置的过滤方法，在此记录一下，以免忘了。</em>

<em>W3School描述如下：</em>
<h3><a name="t0"></a>定义和用法</h3>
<div>

filter_var() 函数通过指定的过滤器过滤变量。

如果成功，则返回已过滤的数据，如果失败，则返回 false。
<h3><a name="t1"></a>语法</h3>
<pre>filter_var(variable, filter, options)</pre>
<table class="dataintable" border="0">
<tbody>
<tr>
<th>参数</th>
<th>描述</th>
</tr>
<tr>
<td>variable</td>
<td>必需。规定要过滤的变量。</td>
</tr>
<tr>
<td>filter</td>
<td>可选。规定要使用的过滤器的 ID。</td>
</tr>
<tr>
<td>options</td>
<td>规定包含标志/选项的数组。检查每个过滤器可能的标志和选项。</td>
</tr>
</tbody>
</table>
</div>
<div>
<h3><a name="t2"></a>提示和注释</h3>
<p class="tip">提示：参见完整的 PHP Filter 参考手册，查看可与该函数一同使用的过滤器。</p>

</div>
<div>
<h3><a name="t3"></a>例子</h3>
<pre>&lt;?php
if(!<code>filter_var("someone@example....com", FILTER_VALIDATE_EMAIL)</code>)
 {
 echo("E-mail is not valid");
 }
else
 {
 echo("E-mail is valid");
 }
?&gt;</pre>
输出类似：
<pre>E-mail is not valid</pre>
<div>
<h2><a name="t4"></a>PHP Filter 简介</h2>
PHP 过滤器用于对来自非安全来源的数据（比如用户输入）进行验证和过滤。

</div>
<div>
<h2><a name="t5"></a>安装</h2>
filter 函数是 PHP 核心的组成部分。无需安装即可使用这些函数。

</div>
<div>
<h2><a name="t6"></a>PHP Filter 函数</h2>
<p class="note">PHP：指示支持该函数的最早的 PHP 版本。</p>

<table class="dataintable" border="0">
<tbody>
<tr>
<th>函数</th>
<th>描述</th>
<th>PHP</th>
</tr>
<tr>
<td><strong>filter_has_var()</strong></td>
<td>检查是否存在指定输入类型的变量。</td>
<td>5</td>
</tr>
<tr>
<td><strong>filter_id()</strong></td>
<td>返回指定过滤器的 ID 号。</td>
<td>5</td>
</tr>
<tr>
<td><strong>filter_input()</strong></td>
<td>从脚本外部获取输入，并进行过滤。</td>
<td>5</td>
</tr>
<tr>
<td><strong>filter_input_array()</strong></td>
<td>从脚本外部获取多项输入，并进行过滤。</td>
<td>5</td>
</tr>
<tr>
<td><strong>filter_list()</strong></td>
<td>返回包含所有得到支持的过滤器的一个数组。</td>
<td>5</td>
</tr>
<tr>
<td><strong>filter_var_array()</strong></td>
<td>获取多项变量，并进行过滤。</td>
<td>5</td>
</tr>
<tr>
<td><strong>filter_var()</strong></td>
<td>获取一个变量，并进行过滤。</td>
<td>5</td>
</tr>
</tbody>
</table>
</div>
<div>
<h2>PHP Filters</h2>
<table class="dataintable" border="0">
<tbody>
<tr>
<th>ID 名称</th>
<th>描述</th>
</tr>
<tr>
<td><strong>FILTER_CALLBACK</strong></td>
<td>调用用户自定义函数来过滤数据。</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_STRING</strong></td>
<td>去除标签，去除或编码特殊字符。</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_STRIPPED</strong></td>
<td>"string" 过滤器的别名。</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_ENCODED</strong></td>
<td>URL-encode 字符串，去除或编码特殊字符。</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_SPECIAL_CHARS</strong></td>
<td>HTML 转义字符 '"&lt;&gt;&amp; 以及 ASCII 值小于 32 的字符。</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_EMAIL</strong></td>
<td>删除所有字符，除了字母、数字以及 !#$%&amp;'*+-/=?^_`{|}~@.[]</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_URL</strong></td>
<td>删除所有字符，除了字母、数字以及 $-_.+!*'(),{}|//^~[]`&lt;&gt;#%";/?:@&amp;=</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_NUMBER_INT</strong></td>
<td>删除所有字符，除了数字和 +-</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_NUMBER_FLOAT</strong></td>
<td>删除所有字符，除了数字、+- 以及 .,eE。</td>
</tr>
<tr>
<td><strong>FILTER_SANITIZE_MAGIC_QUOTES</strong></td>
<td>应用 addslashes()。</td>
</tr>
<tr>
<td><strong>FILTER_UNSAFE_RAW</strong></td>
<td>不进行任何过滤，去除或编码特殊字符。</td>
</tr>
<tr>
<td><strong>FILTER_VALIDATE_INT</strong></td>
<td>在指定的范围以整数验证值。</td>
</tr>
<tr>
<td><strong>FILTER_VALIDATE_BOOLEAN</strong></td>
<td>如果是 "1", "true", "on" 以及 "yes"，则返回 true，如果是 "0", "false", "off", "no" 以及 ""，则返回 false。否则返回 NULL。</td>
</tr>
<tr>
<td><strong>FILTER_VALIDATE_FLOAT</strong></td>
<td>以浮点数验证值。</td>
</tr>
<tr>
<td><strong>FILTER_VALIDATE_REGEXP</strong></td>
<td>根据 regexp，兼容 Perl 的正则表达式来验证值。</td>
</tr>
<tr>
<td><strong>FILTER_VALIDATE_URL</strong></td>
<td>把值作为 URL 来验证。</td>
</tr>
<tr>
<td><strong>FILTER_VALIDATE_EMAIL</strong></td>
<td>把值作为 e-mail 来验证。</td>
</tr>
<tr>
<td><strong>FILTER_VALIDATE_IP</strong></td>
<td>把值作为 IP 地址来验证</td>
</tr>
</tbody>
</table>
</div>
http://php.net/manual/en/function.filter-var.php

</div>
