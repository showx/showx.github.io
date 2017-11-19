---
layout: post
title: js 将json字符串转换为json对象的方法解析
date: 2015-02-06 16:19
author: admin
comments: true
categories: []
---
<div id="art_demo">将json字符串转换为json对象的方法。在数据传输过程中，json是以文本，即字符串的形式传递的，而JS操作的是JSON对象，所以，JSON对象和JSON字符串之间的相互转换是关键</div>
<div class="blank3"></div>
<div id="art_content">

<strong>例如：</strong>

JSON字符串:
var str1 = '{ "name": "cxh", "sex": "man" }';
JSON对象:
var str2 = { "name": "cxh", "sex": "man" };

<strong>一、JSON字符串转换为JSON对象</strong>

要使用上面的str1，必须使用下面的方法先转化为JSON对象：

//由JSON字符串转换为JSON对象

var obj = eval('(' + str + ')');

<strong>或者</strong>

var obj = str.parseJSON(); //由JSON字符串转换为JSON对象

<strong>或者</strong>

var obj = JSON.parse(str); //由JSON字符串转换为JSON对象

然后，就可以这样读取：

Alert(obj.name);

Alert(obj.sex);

特别注意：如果obj本来就是一个JSON对象，那么使用eval（）函数转换后（哪怕是多次转换）还是JSON对象，但是使用parseJSON（）函数处理后会有问题（抛出语法异常）。

<strong>二、可以使用toJSONString()或者全局方法JSON.stringify()将JSON对象转化为JSON字符串。</strong>

<strong>例如：</strong>

var last=obj.toJSONString(); //将JSON对象转化为JSON字符

<strong>或者</strong>

var last=JSON.stringify(obj); //将JSON对象转化为JSON字符

alert(last);

<strong>注意：</strong>

上面的几个方法中，除了eval()函数是js自带的之外，其他的几个方法都来自json.js包。新版本的 JSON 修改了 API，将 JSON.stringify() 和 JSON.parse() 两个方法都注入到了 Javascript 的内建对象里面，前者变成了 Object.toJSONString()，而后者变成了 String.parseJSON()。如果提示找不到toJSONString()和parseJSON()方法，则说明您的json包版本太低。

</div>
