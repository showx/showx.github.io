---
layout: post
title: document.createElement()的用法
date: 2015-08-20 08:39
author: admin
comments: true
categories: []
---
分析代码时，发现自己的盲点——document.createElement()，冲浪一番，总结了点经验。

document.createElement()是在对象中创建一个对象，要与appendChild() 或 insertBefore()方法联合使用。其中，appendChild() 方法在节点的子节点列表末添加新的子节点。insertBefore() 方法在节点的子节点列表任意位置插入新的节点。

下面，举例说明<b>document.createElement()的用法</b>。&lt;div id="board"&gt;&lt;/div&gt;
<b>例1：</b>
&lt;script type="text/javascript"&gt;
var board = document.getElementById("board");
var e = <b>document.createElement</b>("input");
<b>e.type = "button";</b>
e.value = "这是测试加载的小例子";
var object = board.<b>appendChild</b>(e);
&lt;/script&gt;
<b>效果：</b>在标签board中加载一个按钮，属性值为“这是测试加载的小例子”。

<b>例2：</b>
&lt;script type="text/javascript"&gt;
var board = document.getElementById("board");
var e2 = document.createElement("select");
e2.options[0] = new Option("加载项1", "");
e2.options[1] = new Option("加载项2", "");
e2.size = "2";
var object = board.appendChild(e2);
&lt;/script&gt;
<b>效果：</b>在标签board中加载一个下拉列表框，属性值为“加载项1”和“加载项2”。
<b> </b>
<b>例3：</b>
&lt;script type="text/javascript"&gt;
var board = document.getElementById("board");
var e3 = document.createElement("input");
<b>e4.setAttribute("type", "text");</b>
e4.setAttribute("name", "q");
e4.setAttribute("value", "使用setAttribute");
e4.setAttribute("onclick", "javascript:alert('This is a test!');");
var object = board.appendChild(e3);
&lt;/script&gt;
<b>效果：</b>在标签board中加载一个文本框，属性值为“使用setAttribute”。 当点击这个文本框时，会弹出对话框“This is a test!”。

<b>根据上面例子，可以看出，可以通过加载对象的属性来设置，参数是相同的。使用e.type="text" 和 e.setAttribute("type","text")效果是一致的。</b>

<b>下面，我们用实例来讲述一下appendChild() 方法和insertBefore() 方法的不同</b>。
比如我们要在下面这个div中插入一个子节点P时：&lt;div id="test"&gt;&lt;p id="x1"&gt;Node&lt;/p&gt;&lt;p&gt;Node&lt;/p&gt;&lt;/div&gt;
<b>我们可以这样写：</b>
&lt;script type="text/javascript"&gt;
var oTest = document.getElementById("test");
var newNode = document.createElement("p");
newNode.innerHTML = "This is a test";
//测试从这里开始
//appendChild方法：
oTest.appendChild(newNode);
//insertBefore方法：
oTest.insertBefore(newNode,null);
&lt;/script&gt;
通过以上的代码，可以测试到一个新的节点被创建到了节点div下，且该节点是div<b>最后一个节点</b>。很明显，通过这个例子，可以知道appendChildhild和insertBefore都可以进行插入节点的操作。
在上面的例子中有这样一句代码：oTest.insertBefore(newNode,null) ，这里insertBefore有2个参数可以设置，第一个是和appendChild相同的，第二却是它特有的。<b>它不仅可以为null，还可以为：</b>
&lt;script type="text/javascript"&gt;
var oTest = document.getElementById("test");
var refChild = document.getElementById("x1");
var newNode = document.createElement("p");
newNode.innerHTML = "This is a test";
oTest.insertBefore(newNode,refChild);
&lt;/script&gt;
<b>效果：</b>这个例子将在x1节点前面插入一个新的节点

<b>又或：</b>
&lt;script type="text/javascript"&gt;
var oTest = document.getElementById("test");
var refChild = document.getElementById("x1");
var newNode = document.createElement("p");
newNode.innerHTML = "This is a test";
oTest.insertBefore(newNode,refChild.nextSibling);
&lt;/script&gt;
<b>效果：</b>这个例子将在x1节点的下一个节点前面插入一个新的节点

<b>还可为：</b>
&lt;script type="text/javascript"&gt;
var oTest = document.getElementById("test");
var newNode = document.createElement("p");
newNode.innerHTML = "This is a test";
oTest.insertBefore(newNode,oTest.childNodes[0]);
&lt;/script&gt;
这个例子将在第一子节点前面插入一个新的节点，也可以通过改变childNodes[0,1,...]来在其它位置插入新的节点
<b>由于可见insertBefore()方法的特性是在已有的子节点前面插入新的节点</b>，但例一中使用insertBefore()方法也可以在子节点列表末插入新节点的。两种情况结合起来，发现insertBefore()方法插入节点，是可以在子节点列表的任意位置。
从这几个例子中得出：
<b>appendChild() 方法在节点的子节点列表末添加新的子节点。</b>
<b>insertBefore() 方法在节点的子节点列表任意位置插入新的节点。</b>
