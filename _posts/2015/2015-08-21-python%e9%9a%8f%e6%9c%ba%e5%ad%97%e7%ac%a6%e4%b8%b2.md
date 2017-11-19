---
layout: post
title: python随机字符串
date: 2015-08-21 09:12
author: admin
comments: true
categories: []
---
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top"><b>from</b> random <b>import</b> Random</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">2</td>
<td valign="top"><b>def</b> random_str(randomlength<b>=</b>8):</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">3</td>
<td valign="top">    str <b>=</b> ''</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">4</td>
<td valign="top">    chars <b>=</b> 'AaBbCcDdEeFfGgHhIiJjKkLlMmNnOoPpQqRrSsTtUuVvWwXxYyZz0123456789'</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">5</td>
<td valign="top">    length <b>=</b> len(chars) <b>-</b> 1</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">6</td>
<td valign="top">    random <b>=</b> Random()</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">7</td>
<td valign="top">    <b>for</b> i <b>in</b> range(randomlength):</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">8</td>
<td valign="top">        str<b>+=</b>chars[random.randint(0, length)]</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top">9</td>
<td valign="top">    <b>return</b> str</td>
</tr>
</tbody>
</table>
