---
layout: post
title: JavaScript中的apply()方法和call()方法使用介绍
date: 2015-08-10 15:19
author: admin
comments: true
categories: []
---
1、每个函数都包含两个非继承而来的方法：apply()和call()。
2、他们的用途相同，都是在特定的作用域中调用函数。
3、接收参数方面不同，apply()接收两个参数，一个是函数运行的作用域(this)，另一个是参数数组。
call()方法第一个参数与apply()方法相同，但传递给函数的参数必须列举出来。
例1：
<div class="codetitle">代码如下:</div>
<div id="code65809" class="codebody">
window.firstName = "diz";
window.lastName = "song";
var myObject = { firstName: "my", lastName: "Object" };
function HelloName() {
console.log("Hello " + this.firstName + " " + this.lastName, " glad to meet you!");
}
HelloName.call(window); //huo .call(this);
HelloName.call(myObject);</div>
运行结果为：
Hello diz song glad to meet you!
Hello my Object glad to meet you!
例2：
<div class="codetitle">代码如下:</div>
<div id="code57523" class="codebody">
function sum(num1, num2) {
return num1 + num2;
}
console.log(sum.call(window, 10, 10)); //20
console.log(sum.apply(window,[10,20])); //30</div>
分析：在例1中，我们发现apply()和call()的真正用武之地是能够扩充函数赖以运行的作用域，如果我们想用传统的方法实现，请见下面的代码：
<div class="codetitle">代码如下:</div>
<div id="code35497" class="codebody">
window.firstName = "diz";
window.lastName = "song";
var myObject = { firstName: "my", lastName: "Object" };
function HelloName() {
console.log("Hello " + this.firstName + " " + this.lastName, " glad to meet you!");
}
HelloName(); //Hello diz song glad to meet you!
myObject.HelloName = HelloName;
myObject.HelloName(); //Hello my Object glad to meet you!</div>
见加红的代码，我们发现，要想让HelloName()函数的作用域在对象myObject上，我们需要动态创建myObject的HelloName属性，此属性作为指针指向HelloName()函数，这样，当我们调用myObject.HelloName()时，函数内部的this变量就指向myObjecct，也就可以调用该对象的内部其他公共属性了。
通过分析例2，我们可以看到call()和apply()函数的真正运用之处，在实际项目中，还需要根据实际灵活加以处理！
一个小问题：再看一看函数中定义函数时，this变量的情况
<div class="codetitle">代码如下:</div>
<div id="code75017" class="codebody">
function temp1() {
console.log(this); //Object {}
function temp2() {
console.log(this); //Window
}
temp2();
}
var Obj = {};
temp1.call(Obj); //运行结果见上面绿色的注释！！！！</div>
执行结果与下面的相同：
<div class="codetitle">代码如下:</div>
<div id="code56455" class="codebody">
function temp1() {
console.log(this);
temp2();
}
function temp2() {
console.log(this);
}
var Obj = {};
temp1.call(Obj);</div>
<strong>4、bind()方法
</strong>　　支持此方法的浏览器有IE9+、Firefox4+、Safari5.1+、Opera12+、Chrome。它是属于ECMAScript5的方法。直接看例子：
<div class="codetitle">代码如下:</div>
<div id="code38701" class="codebody">
window.color = "red";
var o = { color: "blue" };
function sayColor(){
alert(this.color);
}
var OSayColor = sayColor.bind(o);
OSayColor(); //blue</div>
这里，sayColor()调用bind()方法，并传入o对象，返回了OSayColor()函数，在OSayColor()中，this的值就为o对象
