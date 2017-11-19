---
layout: post
title: Jquery表单插件ajaxForm用法详解
date: 2014-04-17 15:00
author: admin
comments: true
categories: []
---
插件主要的方法：

ajaxForm
ajaxSubmit
formToArray
formSerialize

fieldSerialize
fieldValue
clearForm
clearFields
resetForm

&nbsp;

示例代码：

&nbsp;
<div id="highlighter_321220">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>1</code></td>
<td><code>// wait for the DOM to be loaded</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>2</code></td>
<td><code>$(document).ready(</code><code>function</code><code>() { </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>3</code></td>
<td><code>   </code><code>// bind 'myForm' and provide a simple callback function  </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>4</code></td>
<td><code>   </code><code>$(</code><code>'#myForm'</code><code>).ajaxForm(</code><code>function</code><code>() {  </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>5</code></td>
<td><code>       </code><code>alert(</code><code>"Thank you for your comment!"</code><code>); </code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>6</code></td>
<td><code>    </code><code>});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>7</code></td>
<td><code>});</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
&nbsp;

下载地址： <a href="http://malsup.github.com/jquery.form.js" rel="nofollow">http://malsup.github.com/jquery.form.js</a>

表单插件API

英文原文：<a href="http://www.malsup.com/jquery/form/#api" rel="nofollow">http://www.malsup.com/jquery/form/#api</a>

表单插件API提供了几个方法，让你轻松管理表单数据和进行表单提交。

ajaxForm（）

增加所有需要的事件监听器，为AJAX提交表单做好准备。ajaxForm不能提交表单。在document的ready函数中，使用 ajaxForm来为AJAX提交表单进行准备。ajaxForm接受0个或1个参数。这个单个的参数既可以是一个回调函数，也可以是一个Options 对象。
可链接（Chainable）：可以。

实例：

$('#myFormId').ajaxForm();

ajaxSubmit（）

马上由AJAX来提交表单。大多数情况下，都是调用ajaxSubmit来对用户提交表单进行响应。ajaxSubmit接受0个或1个参数。这个单个的参数既可以是一个回调函数，也可以是一个Options对象。
可链接（Chainable）：可以。

实例：
<div id="highlighter_792337">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>1</code></td>
<td><code>// 绑定表单提交事件处理器</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>2</code></td>
<td><code>$(</code><code>'#myFormId'</code><code>).submit(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>3</code></td>
<td><code>    </code><code>// 提交表单</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>4</code></td>
<td><code>    </code><code>$(</code><code>this</code><code>).ajaxSubmit();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>5</code></td>
<td><code>    </code><code>// 为了防止普通浏览器进行表单提交和产生页面导航（防止页面刷新？）返回false</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>6</code></td>
<td><code>    </code><code>return</code> <code>false</code><code>;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>7</code></td>
<td><code>   </code><code>});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>8</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>9</code></td>
<td><code>formSerialize（）</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
&nbsp;

将表单串行化（或序列化）成一个查询字符串。这个方法将返回以下格式的字符串：name1=value1&amp;name2=value2。
可链接（Chainable）：不能， 这个方法返回一个字符串。

实例：

var queryString = $('#myFormId').formSerialize();

// 现在可以使用$.get、$.post、$.ajax等来提交数据
$.post('myscript.php', queryString);

fieldSerialize（）

将表单的字段元素串行化（或序列化）成一个查询字符串。当只有部分表单字段需要进行串行化（或序列化）时，这个就方便了。这个方法将返回以下格式的字符串：name1=value1&amp;name2=value2。
可链接（Chainable）：不能，这个方法返回一个字符串。

实例：

var queryString = $('#myFormId .specialFields').fieldSerialize();
fieldValue（）

返回匹配插入数组中的表单元素值。从0.91版起，该方法将总是以数组的形式返回数据。如果元素值被判定可能无效，则数组为空，否则它将包含一个或多于一个的元素值。
可链接（Chainable）：不能，该方法返回数组。

实例：

// 取得密码输入值
var value = $('#myFormId :password').fieldValue();
alert('The password is: ' + value[0]);
resetForm（）

通过调用表单元素原有的DOM方法，将表单恢复到初始状态。
可链接（Chainable）：可以。

实例：

$('#myFormId').resetForm();
clearForm（）

清除表单元素。该方法将所有的文本（text）输入字段、密码（password）输入字段和文本区域（textarea）字段置空，清除任何select元素中的选定，以及将所有的单选（radio）按钮和多选（checkbox）按钮重置为非选定状态。
可链接（Chainable）：可以。

$('#myFormId').clearForm();
clearFields（）

清除字段元素。只有部分表单元素需要清除时才方便使用。
可链接（Chainable）：可以。

$('#myFormId .specialFields').clearFields();

Options对象

ajaxForm和ajaxSubmit都支持众多的选项参数，这些选项参数可以使用一个Options对象来提供。Options只是一个JavaScript对象，它包含了如下一些属性与值的集合：

target

指明页面中由服务器响应进行更新的元素。元素的值可能被指定为一个jQuery选择器字符串，一个jQuery对象，或者一个DOM元素。
默认值：null。

url

指定提交表单数据的URL。
默认值：表单的action属性值

type

指定提交表单数据的方法（method）：“GET”或“POST”。
默认值：表单的method属性值（如果没有找到默认为“GET”）。

beforeSubmit

表单提交前被调用的回调函数。“beforeSubmit”回调函数作为一个钩子（hook），被提供来运行预提交逻辑或者校验表单数据。如果 “beforeSubmit”回调函数返回false，那么表单将不被提交。“beforeSubmit”回调函数带三个调用参数：数组形式的表单数据，jQuery表单对象，以及传入ajaxForm/ajaxSubmit中的Options对象。表单数组接受以下方式的数据：

[ { name: 'username', value: 'jresig' }, { name: 'password', value: 'secret' } ]

默认值：null

success

表单成功提交后调用的回调函数。如果提供“success”回调函数，当从服务器返回响应后它被调用。然后由dataType选项值决定传回responseText还是responseXML的值。
默认值：null

dataType

期望返回的数据类型。null、“xml”、“script”或者“json”其中之一。dataType提供一种方法，它规定了怎样处理服务器的响应。这个被直接地反映到jQuery.httpData方法中去。下面的值被支持：

'xml'：如果dataType == 'xml'，将把服务器响应作为XML来对待。同时，如果“success”回调方法被指定， 将传回responseXML值。

'json'：如果dataType == 'json'， 服务器响应将被求值，并传递到“success”回调方法，如果它被指定的话。

'script'：如果dataType == 'script'， 服务器响应将求值成纯文本。

默认值：null（服务器返回responseText值）

semantic

Boolean flag indicating whether data must be submitted in strict semantic order (slower). Note that the normal form serialization is done in semantic order with the exception of input elements of type="image". You should only set the semantic option to true if your server has strict semantic requirements and your form contains an input element of type="image".
布尔标志，表示数据是否必须严格按照语义顺序（slower？）来进行提交。注意：一般来说，表单已经按照语义顺序来进行了串行化（或序列化），除了 type="image"的input元素。如果你的服务器有严格的语义要求，以及表单中包含有一个type="image"的input元素，就应该将 semantic设置为true。（译注：这一段由于无法理解，翻译出来可能语不达意，但请达人指正。）
默认值：false

resetForm

布尔标志，表示如果表单提交成功是否进行重置。
Default value: null

clearForm

布尔标志，表示如果表单提交成功是否清除表单数据。
默认值：null

实例：

// 准备好Options对象
<div id="highlighter_296926">
<div>
<div>
<table>
<tbody>
<tr>
<td><code>1</code></td>
<td><code>var</code> <code>options = {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>2</code></td>
<td><code>    </code><code>target:     </code><code>'#divToUpdate'</code><code>,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>3</code></td>
<td><code>    </code><code>url:        </code><code>'comment.php'</code><code>,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>4</code></td>
<td><code>    </code><code>success: </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>5</code></td>
<td><code>      </code><code>alert(</code><code>'Thanks for your comment!'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>6</code></td>
<td><code>    </code><code>} };</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>7</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>8</code></td>
<td><code>   </code><code>// 将options传给ajaxForm</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table>
<tbody>
<tr>
<td><code>9</code></td>
<td><code>$(</code><code>'#myForm'</code><code>).ajaxForm(options);</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
&nbsp;

&nbsp;
注意：Options对象还可以用来将值传递给jQuery的$.ajax方法。如果你熟悉$.ajax所支持的options，你可以利用它们来将Options对象传递给ajaxForm和ajaxSubmit。

<strong>ajaxForm()</strong>适用于以表单提交方式处理ajax技术（需要提供表单的action、id、 method，最好在表单中提供submit按钮）它大大简化了使用ajax技术提交表单时的数据传递问题，使用ajaxForm()你不需要逐个的以 JavaScript的方式获取每个表单属性的值，并且也不需要在请求路径后面通过url重写的方式传递数据。ajaxForm()会自动收集当前表单中每个属性的值，然后将其以表单提交的方式提交到目标url。这种方式提交数据较安全，并且使用起来更简单，不必写过多冗余的JavaScript代码
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>$(document).ready(function(){

registerForm'表单id

data回调数据

$('#registerForm').ajaxForm(function(data){

alert(data);//弹出ajax请求后的回调结果

});

});</td>
</tr>
</tbody>
</table>
<strong>ajaxSubmit</strong><strong>（）</strong>适用于以事件的机制以ajax提交form表单（超链接、图片的click事件），该方法作用与ajaxForm()类似，但它更为灵活，因为他依赖于事件机制，只要有事件存在就能使用该方法。你只需指定该form的action属性即可，不需要提供submit按钮。
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>$(document).ready(function(){

$('#btn').click(function(){

$('#registerForm').ajaxSubmit(function(data){

alert(data);

});

return false;

});

});

该段代码作用是在表单中id为btn的按钮click事件触发时通过ajaxSubmit（）方法以ajax技术提交表单到表单的action所指路径</td>
</tr>
</tbody>
</table>
<strong>formSerialize</strong><strong>（）</strong>是将一个form中所有的表单元素以 name作为key，value作为值进行序列化操作，这就需要你必须为每一个表单元素设置表单元素name属性及填充表单元素value的值，最好也设置id方便jquery定位表单元素。若要使用此方法你必须设置表单元素name属性及填充表单元素value的值，我在初次使用时就忘了设置name属性，最后在同事的帮助下找了好久才发现此错误。
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>var str=$('#registerForm').formSerialize(); // registerForm为form id

alert(str);</td>
</tr>
</tbody>
</table>
f<strong>ieldSerialize</strong><strong>（）</strong>是将form中表单元素进行序列化以name作为key，value作为值进行序列化操作，这就需要你必须为每一个表单元素设置表单元素name属性及填充表单元素value的值。

var str=$('#username). fieldSerialize();

alert(str);
