---
layout: post
title: jquery如何对多个对象绑定同一事件
date: 2015-08-24 08:53
author: admin
comments: true
categories: []
---
用jquey循环三个元素对象，其中首先利用jquery的选择器选取id为edit的包装集，利用jquery内置的each方法枚举每个对象

同时在对象的事件上绑定函数至，本例使用了点击事件，当事件触发时将屏幕弹出一个简单的窗口。例子：
<ol>
	<li>$("a[id='edit']").each(function (index) {</li>
	<li>      $(this).click(function(){alert('sfsafasdf');});</li>
	<li>   });</li>
	<li></li>
	<li>   $("a[id='add']").each(function (index) {</li>
	<li>      $(this).click(function(){alert('sfsafasdf');});</li>
	<li>   });</li>
</ol>
js关闭窗口

<b>echo</b> "&lt;br&gt;&lt;a onclick='javascript:window.close();' href='javascript:void(0);'&gt;关闭窗口&lt;/a&gt;";
