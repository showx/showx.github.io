---
layout: post
title: jQuery信息提示工具[Poshy Tip]
date: 2015-02-05 10:02
author: admin
comments: true
categories: []
---
Poshy Tip是一款非常友好的信息提示工具，它基于jQuery，当鼠标滑向链接时，会出现一个信息提示条。信息的内容直接可以在HTML里设定也可以是从服务端调用的数据，该插件还提供了很多属性和方法。

&nbsp;

Demo中提供了三种使用的例子，页面代码如下：

&nbsp;
<div class="cnblogs_Highlighter">
<div>
<div id="highlighter_256926" class="syntaxhighlighter nogutter  html">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="html plain">&lt;</code><code class="html keyword">p</code><code class="html plain">&gt;1、&lt;</code><code class="html keyword">a</code> <code class="html plain">id="tip1" title="嗨。。这里有个工具提示条！" href="#"&gt;鼠标滑上这里看看&lt;/</code><code class="html keyword">a</code><code class="html plain">&gt;&lt;/</code><code class="html keyword">p</code><code class="html plain">&gt; </code></div>
<div class="line number2 index1 alt1"><code class="html plain">&lt;</code><code class="html keyword">br</code><code class="html plain">/&gt; </code></div>
<div class="line number3 index2 alt2"><code class="html plain">&lt;</code><code class="html keyword">p</code><code class="html plain">&gt;2、用户名：&lt;</code><code class="html keyword">br</code><code class="html plain">/&gt;&lt;</code><code class="html keyword">input</code> <code class="html plain">id="user" type="text" size="30" title="请输入用户名" /&gt;&lt;/</code><code class="html keyword">p</code><code class="html plain">&gt; </code></div>
<div class="line number4 index3 alt1"><code class="html plain">&lt;</code><code class="html keyword">br</code><code class="html plain">/&gt; </code></div>
<div class="line number5 index4 alt2"><code class="html plain">&lt;</code><code class="html keyword">p</code><code class="html plain">&gt;3、服务端调用:&lt;</code><code class="html keyword">br</code><code class="html plain">/&gt; </code></div>
<div class="line number6 index5 alt1"><code class="html spaces">  </code><code class="html plain">&lt;</code><code class="html keyword">a</code> <code class="html plain">id="remote" href="#"&gt;鼠标滑向这里加载图片&lt;/</code><code class="html keyword">a</code><code class="html plain">&gt; </code></div>
<div class="line number7 index6 alt2"><code class="html plain">&lt;/</code><code class="html keyword">p</code><code class="html plain">&gt; </code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
当然，别忘了要加载jquery库和poshytip插件以及相关样式。
<div class="cnblogs_Highlighter">
<div>
<div id="highlighter_359140" class="syntaxhighlighter nogutter  html">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="html plain">&lt;</code><code class="html keyword">script</code> <code class="html plain">type="text/javascript" src="js/jquery.js"&gt;&lt;/</code><code class="html keyword">script</code><code class="html plain">&gt; </code></div>
<div class="line number2 index1 alt1"><code class="html plain">&lt;</code><code class="html keyword">script</code> <code class="html plain">type="text/javascript" src="src/jquery.poshytip.js"&gt;&lt;/</code><code class="html keyword">script</code><code class="html plain">&gt; </code></div>
<div class="line number3 index2 alt2"><code class="html plain">&lt;</code><code class="html keyword">link</code> <code class="html plain">rel="stylesheet" href="src/tip-yellow/tip-yellow.css" type="text/css" /&gt; </code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<h4>jQuery：</h4>
1、基本使用：
<div class="cnblogs_Highlighter">
<div>
<div id="highlighter_200079" class="syntaxhighlighter nogutter  javascript">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="javascript plain">$(</code><code class="javascript string">"#tip1"</code><code class="javascript plain">).poshytip();</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
2、表单提示：当输入框获得焦点时，在右侧会出现提示工具条
<div class="cnblogs_Highlighter">
<div>
<div id="highlighter_419368" class="syntaxhighlighter nogutter  javascript">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="javascript plain">$(</code><code class="javascript string">'#user'</code><code class="javascript plain">).poshytip({ </code></div>
<div class="line number2 index1 alt1"><code class="javascript spaces">    </code><code class="javascript plain">className: </code><code class="javascript string">'tip-yellowsimple'</code><code class="javascript plain">, </code></div>
<div class="line number3 index2 alt2"><code class="javascript spaces">    </code><code class="javascript plain">showOn: </code><code class="javascript string">'focus'</code><code class="javascript plain">, </code></div>
<div class="line number4 index3 alt1"><code class="javascript spaces">    </code><code class="javascript plain">alignTo: </code><code class="javascript string">'target'</code><code class="javascript plain">, </code></div>
<div class="line number5 index4 alt2"><code class="javascript spaces">    </code><code class="javascript plain">alignX: </code><code class="javascript string">'right'</code><code class="javascript plain">, </code></div>
<div class="line number6 index5 alt1"><code class="javascript spaces">    </code><code class="javascript plain">alignY: </code><code class="javascript string">'center'</code><code class="javascript plain">, </code></div>
<div class="line number7 index6 alt2"><code class="javascript spaces">    </code><code class="javascript plain">offsetX: 5 </code></div>
<div class="line number8 index7 alt1"><code class="javascript plain">});</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
3、服务端调用：通过调用服务端ajax.php，获得返回数据
<div class="cnblogs_Highlighter">
<div>
<div id="highlighter_67159" class="syntaxhighlighter nogutter  javascript">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="javascript plain">$(</code><code class="javascript string">'#remote'</code><code class="javascript plain">).poshytip({ </code></div>
<div class="line number2 index1 alt1"><code class="javascript spaces">    </code><code class="javascript plain">alignY: </code><code class="javascript string">'bottom'</code><code class="javascript plain">, </code></div>
<div class="line number3 index2 alt2"><code class="javascript spaces">    </code><code class="javascript plain">content: </code><code class="javascript keyword">function</code><code class="javascript plain">(updateCallback) { </code></div>
<div class="line number4 index3 alt1"><code class="javascript spaces">        </code><code class="javascript plain">$.get(</code><code class="javascript string">'ajax.php?id=1'</code><code class="javascript plain">,</code><code class="javascript keyword">function</code><code class="javascript plain">(msg){ </code></div>
<div class="line number5 index4 alt2"><code class="javascript spaces">            </code><code class="javascript plain">updateCallback(msg); </code></div>
<div class="line number6 index5 alt1"><code class="javascript spaces">        </code><code class="javascript plain">}); </code></div>
<div class="line number7 index6 alt2"><code class="javascript spaces">        </code><code class="javascript keyword">return</code> <code class="javascript string">'Loading...'</code><code class="javascript plain">; </code></div>
<div class="line number8 index7 alt1"><code class="javascript spaces">    </code><code class="javascript plain">} </code></div>
<div class="line number9 index8 alt2"><code class="javascript plain">}); </code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<h4>参数和方法一览表</h4>
<table class="main_table" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr class="table_title">
<td width="25%">参数/方法</td>
<td>描述</td>
</tr>
<tr>
<td><strong>content</strong></td>
<td>提示工具条中的内容，默认是从元素的title属性中获取。</td>
</tr>
<tr>
<td><strong>className</strong></td>
<td>提示工具条的样式</td>
</tr>
<tr>
<td><strong>showTimeout</strong></td>
<td>提示工具条出现前的过渡时间</td>
</tr>
<tr>
<td><strong>hideTimeout</strong></td>
<td>提示工具条消失的过渡时间</td>
</tr>
<tr>
<td><strong>showOn</strong></td>
<td>提示工具条触发方式，有'hover', 'focus', 'none'三种方式</td>
</tr>
<tr>
<td><strong>alignX</strong></td>
<td>提示工具条出现在水平方向相对当前元素的位置，有'right', 'center', 'left', 'inner-left', 'inner-right'</td>
</tr>
<tr>
<td><strong>alignY</strong></td>
<td>提示工具条出现在垂直方向相对当前元素的位置，有'bottom', 'center', 'top', 'inner-bottom', 'inner-top'</td>
</tr>
<tr>
<td><strong>offsetX</strong></td>
<td>相对X方向位移，数字</td>
</tr>
<tr>
<td><strong>offsetY</strong></td>
<td>相对Y方向位移，数字</td>
</tr>
<tr>
<td><strong>hideTimeout</strong></td>
<td>工具条消失的过渡时间</td>
</tr>
<tr>
<td><strong>hideTimeout</strong></td>
<td>工具条消失的过渡时间</td>
</tr>
<tr>
<td><strong>hideTimeout</strong></td>
<td>工具条消失的过渡时间</td>
</tr>
<tr>
<td><strong>offsetY</strong></td>
<td>相对Y方向位移，数字</td>
</tr>
<tr>
<td><strong>allowTipHover</strong></td>
<td>允许鼠标滑向工具条上方</td>
</tr>
<tr>
<td><strong>fade</strong></td>
<td>是否使用渐隐渐显动画，true/false</td>
</tr>
<tr>
<td><strong>slide</strong></td>
<td>是否使用滑动动画，true/false</td>
</tr>
<tr>
<td><strong>方法：show</strong></td>
<td>.poshytip('show')，手动触发显示提示工具条</td>
</tr>
<tr>
<td><strong>方法：hide</strong></td>
<td>.poshytip('hide')，手动触发隐藏提示工具条</td>
</tr>
<tr>
<td><strong>方法：disable</strong></td>
<td>.poshytip('disable')，手动触发禁用提示工具条</td>
</tr>
<tr>
<td><strong>方法：enable</strong></td>
<td>.poshytip('enable')，手动触发启用提示工具条</td>
</tr>
</tbody>
</table>
