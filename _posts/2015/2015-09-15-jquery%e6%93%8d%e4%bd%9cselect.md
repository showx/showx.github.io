---
layout: post
title: jQuery操作Select
date: 2015-09-15 13:15
author: admin
comments: true
categories: []
---
jQuery是如何控制和操作select的。先看下面的html代码
<div id="codeSnippetWrapper" class="csharpcode-wrapper">
<pre id="codeSnippet" class="csharpcode"><span class="kwrd">&lt;</span><span class="html">select</span> <span class="attr">id</span><span class="kwrd">="test"</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">option</span> <span class="attr">value</span><span class="kwrd">="1"</span><span class="kwrd">&gt;</span>选项一<span class="kwrd">&lt;</span><span class="html">option</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;</span><span class="html">option</span> <span class="attr">value</span><span class="kwrd">="2"</span><span class="kwrd">&gt;</span>选项一<span class="kwrd">&lt;</span><span class="html">option</span><span class="kwrd">&gt;</span>
                          ...
<span class="kwrd">&lt;</span><span class="html">option</span> <span class="attr">value</span><span class="kwrd">="n"</span><span class="kwrd">&gt;</span>选项N<span class="kwrd">&lt;</span><span class="html">option</span><span class="kwrd">&gt;</span>
<span class="kwrd">&lt;/</span><span class="html">select</span><span class="kwrd">&gt;</span></pre>
</div>
所谓jQuery操作“<em><strong>select</strong></em>”, 说的更确切一些是应该是jQuery控制 “<em><strong>option</strong></em>”， 看下面的jQuery代码：
<div id="codeSnippetWrapper" class="csharpcode-wrapper">
<pre class="csharpcode">
//获取第一个option的值
$('#test option:first').val();

//最后一个option的值
$('#test option:last').val();

//获取第二个option的值
$('#test option:eq(1)').val();

//获取选中的值
$('#test').val();
$('#test option:selected').val();

//设置值为2的option为选中状态
$('#test').attr('value','2');

//设置最后一个option为选中
$('#test option:last').attr('selected','selected');
$("#test").attr('value' , $('#test option:last').val());
$("#test").attr('value' , $('#test option').eq($('#test option').length - 1).val());

//获取select的长度
$('#test option').length;

//添加一个option
$("#test").append("<span class="kwrd">&lt;</span><span class="html">option</span> <span class="attr">value</span><span class="kwrd">='n+1'</span><span class="kwrd">&gt;</span>第N+1项<span class="kwrd">&lt;/</span><span class="html">option</span><span class="kwrd">&gt;</span>");
$("<span class="kwrd">&lt;</span><span class="html">option</span> <span class="attr">value</span><span class="kwrd">='n+1'</span><span class="kwrd">&gt;</span>第N+1项<span class="kwrd">&lt;/</span><span class="html">option</span><span class="kwrd">&gt;</span>").appendTo("#test");

//添除选中项
$('#test option:selected').remove();

//删除项选中(这里删除第一项)
$('#test option:first').remove();、

//指定值被删除
$('#test option').each(function(){
    if( $(this).val() == '5'){
         $(this).remove();
     }
});
$('#test option[value=5]').remove();

//获取第一个Group的标签
$('#test optgroup:eq(0)').attr('label');

//获取第二group下面第一个option的值
$('#test optgroup:eq(1) : option:eq(0)').val();</pre>
</div>
