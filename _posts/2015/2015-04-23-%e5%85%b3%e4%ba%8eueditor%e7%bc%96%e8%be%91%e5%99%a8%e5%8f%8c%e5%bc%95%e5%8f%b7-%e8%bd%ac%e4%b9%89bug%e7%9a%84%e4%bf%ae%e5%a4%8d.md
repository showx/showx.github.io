---
layout: post
title: 关于ueditor编辑器双引号 “”转义BUG的修复
date: 2015-04-23 16:53
author: admin
comments: true
categories: []
---
最近在使用ueditor编辑器的时候发现，它会把正常的<strong>&amp;ldquo; 与 &amp;rdquo;</strong>转义为 <strong>&amp;amp;ldquo; &amp;amp;rdquo;</strong>

检查转义的方法发现对于<strong>&amp;ldquo; &amp;rdquo;</strong>是没有做处理的，需要自己加上去

<span id="_xhe_cursor">    ueditor.all.js</span>
unhtml:function (str, reg) {
return str ? str.replace(reg || /[&amp;&lt;"&gt;'](?:(amp|lt|quot|gt|#39|nbsp);)?/g, function (a, b) {
if (b) {
return a;
} else {
return {
'&lt;':'&amp;lt;',
'&amp;':'&amp;amp;',
'"':'&amp;quot;',
'&gt;':'&amp;gt;',
"'":'&amp;#39;'
}[a]
}

}) : '';
},
html:function (str) {
return str ? str.replace(/&amp;((g|l|quo)t|amp|#39|nbsp);/g, function (m) {
return {
'&amp;lt;':'&lt;',
'&amp;amp;':'&amp;',
'&amp;quot;':'"',
'&amp;gt;':'&gt;',
'&amp;#39;':"'",
'&amp;nbsp;':' '
}[m]
}) : '';
},

<strong>修复方法</strong>：

第一个方法 unhtml 把 amp|lt|quot|gt|#39|nbsp 替换为  amp|lt|quot|gt|#39|nbsp|ldquo|rdquo就可以了

第二个方法html

修改成
html:function (str) {
return str ? str.replace(/&amp;((g|l|quo)t|#39|nbsp|ldquo|rdquo|amp);/g, function (m) {
return {
'&amp;lt;':'&lt;',
'&amp;quot;':'"',
'&amp;gt;':'&gt;',
'&amp;#39;':"'",
'&amp;nbsp;':' ',
'&amp;ldquo;':'“',
'&amp;rdquo;':'”',
'&amp;amp;':'&amp;'
}[m]
}) : '';
},

备注

如果替换后无效，请检查你引用的是否是 ueditor.all.min.js

&nbsp;
