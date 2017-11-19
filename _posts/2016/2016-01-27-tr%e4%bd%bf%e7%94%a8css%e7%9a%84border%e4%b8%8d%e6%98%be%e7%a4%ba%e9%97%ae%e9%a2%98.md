---
layout: post
title: TR使用css的border不显示问题
date: 2016-01-27 11:24
author: admin
comments: true
categories: []
---
<div>之前是:</div>
<div>
<div id="codeText" class="codeText">
<ol class="dp-css" start="1">
	<li>&lt;table width="99%" border="0" cellspacing="0" cellpadding="0"&gt;</li>
	<li> &lt;tr style="border:1px solid #f7900f;background-color:#fff1cc;height:25px;"&gt;</li>
	<li>    &lt;td&gt;信息标题、物品类型、游戏/区/服&lt;/td&gt;</li>
	<li>    &lt;td&gt;交易类型&lt;/td&gt;</li>
	<li>    &lt;td&gt;交易状态&lt;/td&gt;</li>
	<li>    &lt;td&gt;价格排序&lt;/td&gt;</li>
	<li>    &lt;td&gt;库存&lt;/td&gt;</li>
	<li>    &lt;td&gt;单价排序&lt;/td&gt;</li>
	<li> &lt;/tr&gt;</li>
	<li>&lt;/table&gt;</li>
</ol>
</div>
</div>
<div>
<table border="0" width="99%" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>信息标题、物品类型、游戏/区/服</td>
<td>交易类型</td>
<td>交易状态</td>
<td>价格排序</td>
<td>库存</td>
<td>单价排序</td>
</tr>
</tbody>
</table>
</div>
<div></div>
<div>使用border-collapse就可以解决了..</div>
<div id="codeText" class="codeText">
<ol class="dp-css" start="1">
	<li>&lt;table width="99%" border="0" cellspacing="0" cellpadding="0" style="border-collapse:collapse;"&gt;</li>
	<li> &lt;tr <span class="Apple-style-span" style="color: #ff0000;">style="border:1px solid #f7900f;background-color:#fff1cc;h</span><span class="Apple-style-span"><span class="Apple-style-span" style="color: #ff0000;">eight:25px;</span></span><span class="Apple-style-span"><span class="Apple-style-span"><span class="Apple-style-span" style="color: #ff0000;">"</span>&gt;</span></span></li>
	<li>    &lt;td&gt;信息标题、物品类型、游戏/区/服&lt;/td&gt;</li>
	<li>    &lt;td&gt;交易类型&lt;/td&gt;</li>
	<li>    &lt;td&gt;交易状态&lt;/td&gt;</li>
	<li>    &lt;td&gt;价格排序&lt;/td&gt;</li>
	<li>    &lt;td&gt;库存&lt;/td&gt;</li>
	<li>    &lt;td&gt;单价排序&lt;/td&gt;</li>
	<li> &lt;/tr&gt;</li>
	<li></li>
</ol>
</div>
<div></div>
<table border="0" width="99%" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>信息标题、物品类型、游戏/区/服</td>
<td>交易类型</td>
<td>交易状态</td>
<td>价格排序</td>
<td>库存</td>
<td>单价排序</td>
</tr>
</tbody>
</table>
<div></div>
<div>如果table的border=1的话很难看的...</div>
<table border="1" width="99%" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>信息标题、物品类型、游戏/区/服</td>
<td>交易类型</td>
<td>交易状态</td>
<td>价格排序</td>
<td>库存</td>
<td>单价排序</td>
</tr>
</tbody>
</table>
<div></div>
&nbsp;
