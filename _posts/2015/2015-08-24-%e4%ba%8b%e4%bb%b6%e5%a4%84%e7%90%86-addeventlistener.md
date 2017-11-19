---
layout: post
title: 事件处理---addEventListener
date: 2015-08-24 08:50
author: admin
comments: true
categories: []
---
<b>事件处理---addEventListener   </b>
addEventListener 用于注册事件处理程序，IE 中为 <a href="http://www.sh-ow.com"><u>attachEvent</u></a>，我们为什么讲 addEventListener 而不讲 attachEvent 呢？一来 attachEvent 比较简单，二来 addEventListener 才是 DOM 中的标准内容。
简介
addEventListener 为文档节点、document、window 或 XMLHttpRequest 注册事件处理程序，在以前我们一般是 &lt;input type="button" onclick="..."，或 document.getElementById("testButton").onclick = FuncName， 而在 DOM 中，我们用 addEventListener（IE 中用 attachEvent）。
语法
target.addEventListener(type, listener, useCapture);
<ul>
	<li>target 文档节点、document、window 或 XMLHttpRequest。</li>
	<li>type 字符串，事件名称，不含“on”，比如“click”、“mouseover”、“keydown”等。</li>
	<li>listener 实现了 EventListener 接口或者是 JavaScript 中的函数。</li>
	<li>useCapture 是否使用捕捉，看了后面的事件流一节后就明白了，一般用 false。</li>
</ul>
示例
function Go()
{
//...
}

document.getElementById("testButton").addEventListener("click", Go, false);
或者 listener 直接就是函数
document.getElementById("testButton").addEventListener("click", function () { ... }, false);



addEventListener－事件流
说到 addEventListener 不得不说到事件流，先说事件流对后面的解释比较方便。
当一个事件发生时，分为三个阶段：
捕获阶段 从根节点开始顺序而下，检测每个节点是否注册了事件处理程序。如果注册了事件处理程序，并且 useCapture 为 true，则调用该事件处理程序。（IE 中无此阶段。）
目标阶段 触发在目标对象本身注册的事件处理程序，也称正常事件派发阶段。
冒泡阶段 从目标节点到根节点，检测每个节点是否注册了事件处理程序，如果注册了事件处理程序，并且 useCapture 为 false，则调用该事件处理程序。
举例
&lt;div id="div1"&gt;
&lt;div id="div2"&gt;
&lt;div id="div3"&gt;
&lt;div id="div4"&gt;
&lt;/div&gt;
&lt;/div&gt;
&lt;/div&gt;
&lt;/div&gt;
如果在 d3 上点击鼠标，事件流是这样的：
捕获阶段 在 div1 处检测是否有 useCapture 为 true 的事件处理程序，若有，则执行该程序，然后再同样地处理 div2。
目标阶段 在 div3 处，发现 div3 就是鼠标点击的节点，所以这里为目标阶段，若有事件处理程序，则执行该程序，这里不论 useCapture 为 true 还是 false。
冒泡阶段 在 div2 处检测是否有 useCapture 为 false 的事件处理程序，若有，则执行该程序，然后再同样地处理 div1。
注意，上述捕获阶段和冒泡阶段中，实际上 div1 之上还应该有结点，比如有 body，但这里不讨论。





addEventListener－第三个参数 useCapture

addEventListener 有三个参数：第一个参数表示事件名称（不含 on，如 "click"）；第二个参数表示要接收事件处理的函数；第三个参数为 useCapture，本文就讲解它。
&lt;div id="outDiv"&gt;
&lt;div id="middleDiv"&gt;
&lt;div id="inDiv"&gt;请在此点击鼠标。&lt;/div&gt;
&lt;/div&gt;
&lt;/div&gt;

&lt;div id="info"&gt;&lt;/div&gt;

var outDiv = document.getElementById("outDiv");
var middleDiv = document.getElementById("middleDiv");
var inDiv = document.getElementById("inDiv");
var info = document.getElementById("info");

outDiv.addEventListener("click", function () { info.innerHTML += "outDiv" + "&lt;br&gt;"; }, false);
middleDiv.addEventListener("click", function () { info.innerHTML += "middleDiv" + "&lt;br&gt;"; }, false);
inDiv.addEventListener("click", function () { info.innerHTML += "inDiv" + "&lt;br&gt;"; }, false);
上述是我们测试的代码，根据 info 的显示来确定触发的顺序，有三个 addEventListener，而 useCapture 可选值为 true 和 false，所以 2*2*2，可以得出 8 段不同的程序。
<ul>
	<li>全为 false 时，触发顺序为：inDiv、middleDiv、outDiv；</li>
	<li>全为 true 时，触发顺序为：outDiv、middleDiv、inDiv；</li>
	<li>outDiv 为 true，其他为 false 时，触发顺序为：outDiv、inDiv、middleDiv；</li>
	<li>middleDiv 为 true，其他为 false 时，触发顺序为：middleDiv、inDiv、outDiv；</li>
	<li>……</li>
</ul>
最终得出如下结论：
<ul>
	<li>true 的触发顺序总是在 false 之前；</li>
	<li>如果多个均为 true，则外层的触发先于内层；</li>
	<li>如果多个均为 false，则内层的触发先于外层。</li>
</ul>
下面提供全部代码，您可以更改其中的 true、false 值，来进行测试。注意，不适用于 IE。



&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "<a href="http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"><u>http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd</u></a>"&gt;
&lt;html xmlns="<a href="http://www.w3.org/1999/xhtml"><u>http://www.w3.org/1999/xhtml</u></a>"&gt;
&lt;head&gt;
&lt;meta http-equiv="Content-Language" content="zh-cn" /&gt;
&lt;meta http-equiv="Content-Type" content="text/html; charset=gb2312" /&gt;
&lt;title&gt;useCapture&lt;/title&gt;
&lt;style type="text/css"&gt;
#outDiv
{
padding:10px 10px 10px 10px;
border:1px solid red;
}
#middleDiv
{
padding:10px 10px 10px 10px;
border:1px solid green;
}
#inDiv
{
padding:10px 10px 10px 10px;
border:1px solid blue;
}
&lt;/style&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;div id="outDiv"&gt;
&lt;div id="middleDiv"&gt;
&lt;div id="inDiv"&gt;请在此点击鼠标。&lt;/div&gt;
&lt;/div&gt;
&lt;/div&gt;
&lt;div id="info"&gt;&lt;/div&gt;
&lt;script language="javascript" type="text/javascript"&gt;
&lt;!--
var outDiv = document.getElementById("outDiv");
var middleDiv = document.getElementById("middleDiv");
var inDiv = document.getElementById("inDiv");
var info = document.getElementById("info");

outDiv.addEventListener("click", function () { info.innerHTML += "outDiv" + "&lt;br&gt;"; }, false);
middleDiv.addEventListener("click", function () { info.innerHTML += "middleDiv" + "&lt;br&gt;"; }, false);
inDiv.addEventListener("click", function () { info.innerHTML += "inDiv" + "&lt;br&gt;"; }, false);
//--&gt;
&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;

addEventListener－event 对象的属性和方法

事件触发时，会将一个 Event 对象传递给事件处理程序，比如：
document.getElementById("testText").addEventListener("keydown", function (event) { alert(event.keyCode); }, false);
事件类型
DOM 事件类型是分为 UIEvent、UIEvent:KeyEvent、UIEvent:MouseEvent，不同的事件有不同的属性和方法，但常用的来说我们都不会用 错，比如我们不会在鼠标事件中去取键盘值（Ctrl、Alt、Shift 除外），所以我们没有必要深究。
该对象的属性和方法有：
view 只读，对象，发生事件的 Window 对象。
type 只读，字符串。比如鼠标点击事件的类型：click。
eventPhase 只读，数字，事件流正经历的阶段。1－捕获，2－目标，3－冒泡。
target 只读，对象，派发事件的目标对象。比如鼠标是点击在哪个按钮上的。
currentTarget 只读，对象，当前正在调用监听器的对象，也就是当前 addEventListener 是绑定在哪个对象上的。
timeStamp 只读，数字，用毫秒表示事件发生时距计算机开机的时间。


cancelable 只读，布尔，处理事件的默认行为是否可以停止。主要针对一些系统事件，如果值为 true，则 event 的 preventDefault 方法可以使用，否则不可用。
preventDefault() 阻止浏览器的默认行为，比如在文本框中打字触发 keydown，如果 keydown 事件处理程序中调用了 preventDefault()，所打的字就不会跑到文本框中去，注意，此时不要弹出 alert 对话框，否则可能不起作用。IE 中在事件处理程序中用 return false 实现类似功能。


bubbles 只读，布尔，事件是否开启冒泡功能。
stopImmediatePropagation 这个东西在 JavaScript 中是个属性，而不是方法，布尔，但具体测试并未发现其用途，不知是不是 bug。
stopPropagation() 停止当前的事件流传播，但不会停止当前正在处理的对象。IE 中用 event.cancelBubble =  true 实现类似功能。
cancelBubble 布尔，是否取消冒泡，不建议使用，用 stopPropagation() 代替。
preventBubble() 阻止冒泡，不建议使用，用 stopPropagation() 代替。
preventCapture() 阻止捕获，不建议使用，用 stopPropagation() 代替。


detail 只读，数字，提供时间的额外信息，对于 click 事件、mousedown 事件和 mouseup 事件，这个字段代表点击的次数。
isChar 只读，布尔，按下的按键值是否是字符，比如按下 Ctrl 键时，就返回 false。不过您在 Firefox 中测试时，该值总是 false，Firefox 官方已经说明这是一个 bug。
altKey 只读，布尔，是否按下了 Alt 键。
ctrlKey 只读，布尔，是否按下了 Ctrl 键。
shiftKey 只读，布尔，是否按下了 Shift 键。
metaKey 只读，布尔，是否按下了 Meta 键。


下面一些属性很有意思，请仔细区别。
charCode 只读，数字，字符（英文、数字、符号）的 Unicode 值。
<ul>
	<li>只用于 keypress。</li>
</ul>
keyCode 只读，数字，键盘按键值。
<ul>
	<li>用于 keypress 时：返回非字符按键值（除 Ctrl、Shift、Alt、Caps Lock、单行文本框中按向上键等）；</li>
	<li>用于 keydown、keyup 时：返回任意键值。</li>
</ul>
button 只读，数字，鼠标按键值。
<ul>
	<li>用于 click 时：0－左键。</li>
	<li>用于 mousedown、mouseup 时：0－左键，1－中间键（滚轮），2－右键。</li>
</ul>
which 只读，数字，键盘按键值或鼠标按键值。
<ul>
	<li>用于 keypress 时：等同于 charCode + 回退键 + 回车键；</li>
	<li>用于 keydown、keyup 时：返回任意键值；</li>
	<li>用于 click 时：1－左键，与 button 的值略有区别。</li>
	<li>用于 mousedown、mouseup 时：1－左键，2－中间键（滚轮），3－右键，与 button 的值略有区别。</li>
</ul>
可以看出，which 只有一点没有包括：那就是 keypress 时，不如 keyCode 那么全，但实际上，keypress 事件中用于非字符键的情况较少，所以一般还是用 which 代替全部。


addEventListener－有用的笔记

为什么用 addEventListener
<ul>
	<li>可以对同一物件的同一事件绑定多个事件处理程序。</li>
	<li>可以通过事件流三个阶段更好地控制何时触发事件处理程序。</li>
	<li>工作于 DOM 元素，而不仅是 HTML 元素。</li>
</ul>
事件分发时添加 eventListener
不会立即触发 eventListener，可能会在下一个事件流（比如冒泡阶段）中触发。
多个相同的 eventListener
如下，三个参数完全相同，并且第二个参数不是匿名函数。
document.getElementById("myBox").addEventListener("click", Go, false);
document.getElementById("myBox").addEventListener("click", Go, false);
document.getElementById("myBox").addEventListener("click", Go, false);
会抛弃多余的，只保留一个，对应的 removeEventListener 也只用一次就可以了（removeEventListener 用法和 addEventListener 完全相同）。
但如果是第二个参数是匿名函数，比如：
document.getElementById("outDiv").addEventListener("click", function () { document.getElementById("info").innerHTML += "1";}, false);
document.getElementById("outDiv").addEventListener("click", function () { document.getElementById("info").innerHTML += "1";}, false);
document.getElementById("outDiv").addEventListener("click", function () { document.getElementById("info").innerHTML += "1";}, false);
则三个均有效，并且无法用 removeEventListener 除去。
this
事件处理程序中，this 变成了触发事件的控件，但我们仍推荐用 event.target 或 event.currentTarget。
早期的事件监听
在 DOM0 中，我们用 obj.onclick = FuncName，由于兼容性好，应用非常广泛，只是功能不如 addEventListener 强大。
内存问题
前面提到了许多使用域名函数的地方，有时这是没办法的，但这会导致内存问题。
一旦事件绑定之后，该绑定代码作用域的变量就都保留下来，不会被 JavaScript 引擎回收，这可能会引起占用大量内存的问题，由于 removeEventListener 无法删除匿名函数的事件处理程序，只有在物件（比如按钮）去除之后，该内存才可能得到回收。
