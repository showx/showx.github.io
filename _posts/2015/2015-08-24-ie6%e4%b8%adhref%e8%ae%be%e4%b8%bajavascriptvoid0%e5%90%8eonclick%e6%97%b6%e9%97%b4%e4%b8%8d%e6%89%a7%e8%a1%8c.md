---
layout: post
title: ie6中href设为javascript:void(0)后onclick时间不执行
date: 2015-08-24 08:50
author: admin
comments: true
categories: []
---
在做一个项目的时候，给a的href设为javascript:void(0)后再加onclick事件不执行，在FF,IE7下没有问题.
看来是因为 ie6 执行默认动作引起来，目前两种解决方法：
<b>第一种方法：</b>
<b>&lt;a</b>‍ class="bt_3"  style="cursor:pointer;"‍ id="btnSubmit1" onclick="submitPage()"<b>&gt;</b>提交<b>&lt;/a&gt;</b>
&lt;a class="bt_3" style="cursor:pointer;" id="btnSubmit1" onclick="submitPage()"&gt;提交&lt;/a&gt;
这种方法根本没有href属性，用style="cursor:pointer;" 产生手型图标来模拟。
<b>另一种方法：</b>
<b>&lt;a</b>‍ class="bt_3"   href="javascript:void(0)"  id="btnSubmit1" onclick="submitPage();return false;"<b>&gt;</b>提交<b>&lt;/a&gt;</b>
onclick 返回 false ,阻止浏览器的默认行为。也可以达到相同的目的。
<b>延伸阅读：</b>
让我们先来看看JavaScript中void(0)的含义：
JavaScript中void是一个操作符，该操作符指定要计算一个表达式但是不返回值。

<b>void 操作符用法格式如下：</b>
javascript:void (expression_r)
javascript:void expression_r

expression_r是一个要计算的 JavaScript 标准的表达式。
表达式外侧的圆括号是可选的，但是写上去是一个好习惯。
我们可以使用 void 操作符指定超级链接。表达式会被计算但是不会在当前文档处装入任何内容。
也就是说，要执行某些处理，但是不整体刷新页面的情况下，可以使用void(0),但是在需要对页面进行refresh的情况下，那就要仔细了。
Html代码
&lt;script type="text/javascript"&gt;
function goUrl(x){
window.location.href=x;
}
&lt;/script&gt;
&lt;a href="javascript:;" onclick="javascript:goUrl('http://www.sina.com');"&gt;跳转1&lt;/a&gt;

&lt;a href="javascript:void(0);" onclick="javascript:goUrl('http://www.sina.com');"&gt;跳转2&lt;/a&gt;

&lt;a href="javascript:void(0);" onclick="javascript:goUrl('http://www.sina.com');return false;"&gt;跳转3&lt;/a&gt;

&lt;a href="#" onclick="javascript:goUrl('http://www.sina.com');"&gt;跳转4&lt;/a&gt;

&lt;a href="###" onclick="javascript:goUrl('http://www.sina.com');"&gt;跳转5&lt;/a&gt;

测试环境IE6,IE7,Firefox 3。

跳转1和2在IE6环境下无效，3、4、5在IE6,IE7,Firefox3.01下测试均能 通过，。

跳转4和5最简洁。

关键在于&lt;a&gt;的href属性,空链接用"#","###"。

为了不返回网页顶端。

空链接推荐用"###"。
