---
layout: post
title: JQuery选择器及radio，checkbox,select取值和反选
date: 2015-09-07 15:56
author: admin
comments: true
categories: []
---
<strong>jQuery 的选择器可谓之强大无比，这里简单地总结一下常用的元素查找方法</strong>

$("#myELement")    选择id值等于myElement的元素，id值不能重复在文档中只能有一个id值是myElement所以得到的是唯一的元素

$("div")           选择所有的div标签元素，返回div元素数组

$(".myClass")      选择使用myClass类的css的所有元素

$("*")             选择文档中的所有的元素，可以运用多种的选择方式进行联合选择：例如$("#myELement,div,.myclass")

&nbsp;

<strong>层叠选择器： </strong>

$("form input")         选择所有的form元素中的input元素

$("#main &gt; *")          选择id值为main的所有的子元素

$("label + input")     选择所有的label元素的下一个input元素节点，经测试选择器返回的是label标签后面直接跟一个input标签的所有input标签元素

$("#prev ~ div")       同胞选择器，该选择器返回的为id为prev的标签元素的所有的属于同一个父元素的div标签

&nbsp;

<strong>基本过滤选择器：</strong>

$("tr:first")               选择所有tr元素的第一个

$("tr:last")                选择所有tr元素的最后一个

$("input:not(:checked) + span")

过滤掉：checked的选择器的所有的input元素

$("tr:even")               选择所有的tr元素的第0，2，4... ...个元素（注意：因为所选择的多个元素时为数组，所以序号是从0开始）

$("tr:odd")                选择所有的tr元素的第1，3，5... ...个元素

$("td:eq(2)")             选择所有的td元素中序号为2的那个td元素

$("td:gt(4)")             选择td元素中序号大于4的所有td元素

$("td:lt(4)")              选择td元素中序号小于4的所有的td元素

$(":header")            选择h1、h2、h3之类的

$("div:animated")     选择正在执行动画效果的元素

&nbsp;

<strong>内容过滤选择器：</strong>

$("div:contains('John')") 选择所有div中含有John文本的元素

$("td:empty")           选择所有的为空（也不包括文本节点）的td元素的数组

$("div:has(p)")        选择所有含有p标签的div元素

$("td:parent")          选择所有的以td为父节点的元素数组

&nbsp;

<strong>可视化过滤选择器：</strong>

$("div:hidden")        选择所有的被hidden的div元素

$("div:visible")        选择所有的可视化的div元素

&nbsp;

<strong>属性过滤选择器：</strong>

$("div[id]")              选择所有含有id属性的div元素

$("input[name='newsletter']")    选择所有的name属性等于'newsletter'的input元素

$("input[name!='newsletter']") 选择所有的name属性不等于'newsletter'的input元素

$("input[name^='news']")         选择所有的name属性以'news'开头的input元素

$("input[name$='news']")         选择所有的name属性以'news'结尾的input元素

$("input[name*='man']")          选择所有的name属性包含'news'的input元素

$("input[id][name$='man']")    可以使用多个属性进行联合选择，该选择器是得到所有的含有id属性并且那么属性以man结尾的元素

&nbsp;

<strong>子元素过滤选择器：</strong>

$("ul li:nth-child(2)"),$("ul li:nth-child(odd)"),$("ul li:nth-child(3n + 1)")

$("div span:first-child")          返回所有的div元素的第一个子节点的数组

$("div span:last-child")           返回所有的div元素的最后一个节点的数组

$("div button:only-child")       返回所有的div中只有唯一一个子节点的所有子节点的数组

&nbsp;

<strong>表单元素选择器：</strong>

$(":input")                  选择所有的表单输入元素，包括input, textarea, select 和 button

$(":text")                     选择所有的text input元素

$(":password")           选择所有的password input元素

$(":radio")                   选择所有的radio input元素

$(":checkbox")            选择所有的checkbox input元素

$(":submit")               选择所有的submit input元素

$(":image")                 选择所有的image input元素

$(":reset")                   选择所有的reset input元素

$(":button")                选择所有的button input元素

$(":file")                     选择所有的file input元素

$(":hidden")               选择所有类型为hidden的input元素或表单的隐藏域

&nbsp;

<strong>表单元素过滤选择器：</strong>

$(":enabled")             选择所有的可操作的表单元素

$(":disabled")            选择所有的不可操作的表单元素

$(":checked")            选择所有的被checked的表单元素

$("select option:selected") 选择所有的select 的子元素中被selected的元素

&nbsp;

选取一个 name 为”S_03_22″的input text框的上一个td的text值

$(”input[@ name =S_03_22]“).parent().prev().text()

&nbsp;

名字以”S_”开始，并且不是以”_R”结尾的

$(”input[@ name ^='S_']“).not(”[@ name $='_R']“)

&nbsp;

一个名为 radio_01的radio所选的值

$(”input[@ name =radio_01][@checked]“).val();

&nbsp;

$("A B") 查找A元素下面的所有子节点，包括非直接子节点

$("A&gt;B") 查找A元素下面的直接子节点

$("A+B") 查找A元素后面的兄弟节点，包括非直接子节点

$("A~B") 查找A元素后面的兄弟节点，不包括非直接子节点

&nbsp;

<strong>1. $("A B") 查找A元素下面的所有子节点，包括非直接子节点</strong>

例子：找到表单中所有的 input 元素

&nbsp;
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Html代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-xml" start="1">
	<li><span class="tag">&lt;</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">label</span><span class="tag">&gt;</span>Name:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"name"</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">fieldset</span><span class="tag">&gt;</span></li>
	<li>      <span class="tag">&lt;</span><span class="tag-name">label</span><span class="tag">&gt;</span>Newsletter:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li>      <span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"newsletter"</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">fieldset</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"none"</span> <span class="tag">/&gt;</span><span class="tag">&lt;</span><span class="tag-name">span</span> <span class="attribute">style</span>=<span class="attribute-value">"font-family: verdana, 'courier new'; font-size: medium;"</span> <span class="attribute">size</span>=<span class="attribute-value">"4"</span> <span class="attribute">face</span>=<span class="attribute-value">"verdana, 'courier new'"</span><span class="tag">&gt;</span><span class="tag">&lt;</span><span class="tag-name">span</span> <span class="attribute">style</span>=<span class="attribute-value">"font-size: 14px; line-height: 21px; white-space: normal;"</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">span</span><span class="tag">&gt;</span><span class="tag">&lt;/</span><span class="tag-name">span</span><span class="tag">&gt;</span></li>
</ol>
</div>
&nbsp;

jQuery 代码:

$("form input")

结果:

[ &lt;input name="name" /&gt;, &lt;input name="newsletter" /&gt; ]

&nbsp;

&nbsp;

<strong>2. $("A&gt;B") 查找A元素下面的直接子节点 </strong>

例子：匹配表单中所有的子级input元素。

&nbsp;
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Html代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-xml" start="1">
	<li><span class="tag">&lt;</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">label</span><span class="tag">&gt;</span>Name:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"name"</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">fieldset</span><span class="tag">&gt;</span></li>
	<li>      <span class="tag">&lt;</span><span class="tag-name">label</span><span class="tag">&gt;</span>Newsletter:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li>      <span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"newsletter"</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">fieldset</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"none"</span> <span class="tag">/&gt;</span><span class="tag">&lt;</span><span class="tag-name">span</span> <span class="attribute">style</span>=<span class="attribute-value">"font-family: verdana, 'courier new'; font-size: medium;"</span> <span class="attribute">size</span>=<span class="attribute-value">"4"</span> <span class="attribute">face</span>=<span class="attribute-value">"verdana, 'courier new'"</span><span class="tag">&gt;</span><span class="tag">&lt;</span><span class="tag-name">span</span> <span class="attribute">style</span>=<span class="attribute-value">"font-size: 14px; line-height: 21px; white-space: normal;"</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">span</span><span class="tag">&gt;</span><span class="tag">&lt;/</span><span class="tag-name">span</span><span class="tag">&gt;</span></li>
</ol>
</div>
&nbsp;

jQuery 代码:

$("form &gt; input")

结果:

[ &lt;input name="name" /&gt; ]

&nbsp;

<strong>3. $("A+B") 查找A元素后面的兄弟节点，包括非直接子节点 </strong>

例子：匹配所有跟在 label 后面的 input 元素

&nbsp;
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Html代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-xml" start="1">
	<li><span class="tag">&lt;</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">label</span><span class="tag">&gt;</span>Name:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"name"</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">fieldset</span><span class="tag">&gt;</span></li>
	<li>      <span class="tag">&lt;</span><span class="tag-name">label</span><span class="tag">&gt;</span>Newsletter:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li>      <span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"newsletter"</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">fieldset</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"none"</span> <span class="tag">/&gt;</span></li>
</ol>
</div>
&nbsp;

jQuery 代码:

$("label + input")

结果:

[ &lt;input name="name" /&gt;, &lt;input name="newsletter" /&gt; ]

&nbsp;

<strong>4. $("A~B") 查找A元素后面的兄弟节点，不包括非直接子节点 </strong>

例子：找到所有与表单同辈的 input 元素

&nbsp;
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Html代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-xml" start="1">
	<li><span class="tag">&lt;</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">label</span><span class="tag">&gt;</span>Name:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"name"</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">fieldset</span><span class="tag">&gt;</span></li>
	<li>      <span class="tag">&lt;</span><span class="tag-name">label</span><span class="tag">&gt;</span>Newsletter:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li>      <span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"newsletter"</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">fieldset</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">name</span>=<span class="attribute-value">"none"</span> <span class="tag">/&gt;</span> <span class="tag">&lt;</span><span class="tag-name">span</span> <span class="attribute">style</span>=<span class="attribute-value">"font-family: verdana, 'courier new'; font-size: medium;"</span> <span class="attribute">size</span>=<span class="attribute-value">"4"</span> <span class="attribute">face</span>=<span class="attribute-value">"verdana, 'courier new'"</span><span class="tag">&gt;</span><span class="tag">&lt;</span><span class="tag-name">span</span> <span class="attribute">style</span>=<span class="attribute-value">"font-size: 14px; line-height: 21px; white-space: normal;"</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">span</span><span class="tag">&gt;</span><span class="tag">&lt;/</span><span class="tag-name">span</span><span class="tag">&gt;</span></li>
</ol>
</div>
&nbsp;

jQuery 代码:

$("form ~ input")

结果:

[ &lt;input name="none" /&gt; ]

&nbsp;

<strong>1. 使用jquery获取radio的值</strong>

&nbsp;

使用<strong>jquery获取radio的值</strong> ,最重要的是掌握<a href="http://www.jquerycn.cn/">jquery</a> 选择器的使用，在一个表单中我们通常是要获取被选中的那个radio项的值，所以要加checked来筛选，比如有以下的一些radio项：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Html代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-xml" start="1">
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">type</span>=<span class="attribute-value">"radio"</span> <span class="attribute">name</span>=<span class="attribute-value">"testradio"</span> <span class="attribute">value</span>=<span class="attribute-value">"jquery获取radio的值"</span> <span class="tag">/&gt;</span>jquery获取radio的值<span class="tag">&lt;</span><span class="tag-name">br</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">type</span>=<span class="attribute-value">"radio"</span> <span class="attribute">name</span>=<span class="attribute-value">"testradio"</span> <span class="attribute">value</span>=<span class="attribute-value">"jquery获取checkbox的值"</span> <span class="tag">/&gt;</span>jquery获取checkbox的值<span class="tag">&lt;</span><span class="tag-name">br</span> <span class="tag">/&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">type</span>=<span class="attribute-value">"radio"</span> <span class="attribute">name</span>=<span class="attribute-value">"testradio"</span> <span class="attribute">value</span>=<span class="attribute-value">"jquery获取select的值"</span> <span class="tag">/&gt;</span>jquery获取select的值<span class="tag">&lt;</span><span class="tag-name">br</span> <span class="tag">/&gt;</span></li>
</ol>
</div>
&nbsp;

要想获取某个radio的值有以下的几种方法,直接给出代码：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed title="undefined" src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[name="testradio"]:checked'</span>).val();</li>
</ol>
</div>
&nbsp;
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input:radio:checked'</span>).val();</li>
</ol>
</div>
&nbsp;
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[@name="testradio"][checked]'</span>);</li>
</ol>
</div>
&nbsp;
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[name="testradio"]'</span>).filter(<span class="string">':checked'</span>);</li>
</ol>
</div>
&nbsp;

差不多挺全的了，如果我们要遍历name为testradio的所有radio呢，代码如下
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[name="testradio"]'</span>).each(<span class="keyword">function</span>(){</li>
	<li>            alert(<span class="keyword">this</span>.value);</li>
	<li>        });</li>
</ol>
</div>
&nbsp;

如果要取具体某个radio的值，比如第二个radio的值，这样写
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[name="testradio"]:eq(1)'</span>).val()</li>
</ol>
</div>
&nbsp;

通过修改运行下面的实例，加深你的印象
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>&lt;html&gt;</li>
	<li>&lt;head&gt;</li>
	<li>&lt;script type=<span class="string">"text/javascript"</span> src=<span class="string">"https://ajax.googleapis.com/ajax/libs/jquery/1.7.0/jquery.min.js"</span>&gt;&lt;/script&gt;</li>
	<li>&lt;script type=<span class="string">"text/javascript"</span>&gt;</li>
	<li>$(<span class="keyword">function</span>(){</li>
	<li>    $(<span class="string">'#go'</span>).click(<span class="keyword">function</span>(){</li>
	<li>        <span class="keyword">var</span> radio = $(<span class="string">'input[name="testradio"]'</span>).filter(<span class="string">':checked'</span>);</li>
	<li>        <span class="keyword">if</span>(radio.length)</li>
	<li>        alert(radio.val());</li>
	<li>        <span class="keyword">else</span></li>
	<li>        alert(<span class="string">'请选择一个radio'</span>);</li>
	<li></li>
	<li>    });</li>
	<li>    $(<span class="string">'#go2'</span>).click(<span class="keyword">function</span>(){</li>
	<li>        $(<span class="string">'input[name="testradio"]'</span>).each(<span class="keyword">function</span>(){</li>
	<li>            alert(<span class="keyword">this</span>.value);</li>
	<li>        });</li>
	<li>    })</li>
	<li>    $(<span class="string">'#go3'</span>).click(<span class="keyword">function</span>(){</li>
	<li>        alert($(<span class="string">'input[name="testradio"]:eq(1)'</span>).val());</li>
	<li>    })</li>
	<li>})</li>
	<li>&lt;/script&gt;</li>
	<li></li>
	<li>&lt;/head&gt;</li>
	<li></li>
	<li>&lt;body&gt;</li>
	<li>&lt;input type=<span class="string">"radio"</span> name=<span class="string">"testradio"</span> value=<span class="string">"jquery获取radio的值"</span> /&gt;jquery获取radio的值&lt;br /&gt; &lt;input type=<span class="string">"radio"</span> name=<span class="string">"testradio"</span> value=<span class="string">"jquery获取checkbox的值"</span> /&gt;jquery获取checkbox的值&lt;br /&gt; &lt;input type=<span class="string">"radio"</span> name=<span class="string">"testradio"</span> value=<span class="string">"jquery获取select的值"</span> /&gt;jquery获取select的值&lt;br /&gt;</li>
	<li>&lt;button id=<span class="string">"go"</span>&gt;选中的那个radio的值&lt;/button&gt;</li>
	<li>&lt;button id=<span class="string">"go2"</span>&gt;遍历所有radio的值&lt;/button&gt;</li>
	<li>&lt;button id=<span class="string">"go3"</span>&gt;取第二个radio的值&lt;/button&gt;</li>
	<li>&lt;/body&gt;</li>
	<li>&lt;/html&gt;</li>
</ol>
</div>
&nbsp;

来源：http://www.jquerycn.cn/jquery-tutorial/selector-88.html

&nbsp;

<strong>2. jquery获取checkbox的操作总结</strong>

&nbsp;

使用<strong><a href="http://www.jquerycn.cn/jquery-tutorial/selector-94.html" target="_blank">jquery 获取checkbox</a> </strong>一般使用 name 来获取，因为在 form 表单中，同一组的 checkbox 的 name 是相同的，所以我们可以通过下面的代码来获取 checkbox
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[name="demo"]:checkbox'</span>);</li>
</ol>
</div>
&nbsp;

意思是要获取 name 为 demo 的所有 checkbox 选项，如果我们要将其选中可以这样写：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[name="demo"]:checkbox'</span>).attr(<span class="string">'checked'</span>,<span class="string">'true'</span>);</li>
</ol>
</div>
&nbsp;

也就是将这个 checkbox 元素的 checked 属性的值设为 true，如果你对 <a href="http://www.jquerycn.cn/jquery-tutorial/attr-80.html">jquery设置属性</a> 值不明白，可以查看<a href="http://www.jquerycn.cn/jquery-tutorial/attr-80.html">http://www.jquerycn.cn/jquery-tutorial/attr-80.html</a>

由于我们通常获取 checkbox 获取的是多个，如果我们要获取选中的 checkbox 的值，就要确定是要获取哪个 checkbox 的值，如果这样写：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[name="demo"]:checked'</span>).val()</li>
</ol>
</div>
&nbsp;

这样写是获取了所有选中的 checkbox 中第一个 checkbox 的值，如果要获取所有的 checkbox 的值，我们可以用 eq() 方法来获取每一个的值，比如：
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="string">'input[name="demo"]:checked'</span>).eq(0).val();</li>
	<li>$(<span class="string">'input[name="demo"]:checked'</span>).eq(1).val();</li>
</ol>
</div>
&nbsp;

当然你还可以添加更多的筛选项来，个性化的获取想要的checkbox，比如 :even，:odd 筛选项来获取，第奇数个或第偶数个 checkbox 想，总是 jquery 获取 checkbox 还是很方便的

&nbsp;

来源：http://www.jquerycn.cn/jquery-tutorial/selector-94.html

&nbsp;

<strong>3. jquery获取select值的方法总结</strong>

&nbsp;

<strong><a href="http://www.jquerycn.cn/jquery-tutorial/selector-95.html" target="_blank">jquery 获取select值</a> </strong>的情况有两种：一种是获得 select 的被选中的那个 option 的 value值，一种是获得 select 的被选中的那个 option 的 innerHTML（即包含在&lt;option&gt;&lt;/option&gt;中的内容）

&nbsp;

当然要用 jquery获取select值 就要先获取 select 的<a href="http://www.jquerycn.cn/">jQuery</a> 对象，有以下几种方法：
1.通过 select 的 Id 来获取，如 $('#select_id')
2.通过 select 的 name 来获取，如$('select[name="select_name"]')
当然获取 select 元素的 jQuery 对象还有很多方法，这里就不一一列举了，下面的代码都是用来获取 value 值或 text 值的

&nbsp;

一、jquery 获取select的值，也就是被选中的那个 option 的 value 属性的值
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li><span class="comment">//通过 select 的 id</span></li>
	<li>$(<span class="string">'#select_id option:selected'</span>).val();</li>
	<li>$(<span class="string">'#select_id'</span>).find(<span class="string">'option:selected'</span>).val();</li>
	<li><span class="comment">//或者用原生的方式</span></li>
	<li>$(<span class="string">'#select_id option:selected'</span>)[0].value;</li>
	<li><span class="comment">//通过 select 的 name</span></li>
	<li>$(<span class="string">'select[name="select_name"] option:selected'</span>).val();</li>
	<li>$(<span class="string">'select[name="select_name"]'</span>).find(<span class="string">'option:selected'</span>).val();</li>
</ol>
</div>
&nbsp;

二、jquery获取select被选中的那个 option 的 innerHTML 值（即text值，也就是在&lt;option&gt;&lt;/option&gt;中间的内容）
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li><span class="comment">//通过 select 的 id</span></li>
	<li>$(<span class="string">'#select_id option:selected'</span>).text();</li>
	<li>$(<span class="string">'#select_id'</span>).find(<span class="string">'option:selected'</span>).text();</li>
	<li><span class="comment">//或者用原生的方式</span></li>
	<li>$(<span class="string">'#select_id option:selected'</span>)[0].innerHTML;</li>
	<li><span class="comment">//通过 select 的 name</span></li>
	<li>$(<span class="string">'select[name="select_name"] option:selected'</span>).text();</li>
	<li>$(<span class="string">'select[name="select_name"]'</span>).find(<span class="string">'option:selected'</span>).text();</li>
</ol>
</div>
&nbsp;

好了，jquery 获取 select 值的内容就先到这里了，你也可以看看<a href="http://www.jquerycn.cn/jquery-tutorial/attr-89.html">jquery select选中</a> option 的文章
<a href="http://www.jquerycn.cn/jquery-tutorial/attr-89.html">http://www.jquerycn.cn/jquery-tutorial/attr-89.html</a>

&nbsp;

来源： http://www.jquerycn.cn/jquery-tutorial/selector-95.html

&nbsp;

<strong>反选radio</strong>

&nbsp;
<div class="post-text">

either (plain js)
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li><span class="keyword">this</span>.checked = <span class="keyword">false</span>;</li>
</ol>
</div>
&nbsp;

or (jQuery)
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Js代码 <embed src="http://justcoding.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://justcoding.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-c" start="1">
	<li>$(<span class="keyword">this</span>).prop(<span class="string">'checked'</span>, <span class="keyword">false</span>);</li>
	<li><span class="comment">// Note that the pre-jQuery 1.6 idiom was</span></li>
	<li><span class="comment">// $(this).attr('checked', false);</span></li>
</ol>
</div>
&nbsp;

See <a href="http://api.jquery.com/prop/" target="_blank"><strong>jQuery prop() help page</strong></a> for an explanation on the difference between <em>attr()</em> and <em>prop()</em> and why prop() is now preferable.
prop() was introduced with jQuery 1.6 in May 2011.

</div>
