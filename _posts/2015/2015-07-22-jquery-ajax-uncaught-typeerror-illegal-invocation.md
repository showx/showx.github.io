---
layout: post
title: jquery ajax “Uncaught TypeError: Illegal invocation”
date: 2015-07-22 10:15
author: admin
comments: true
categories: []
---
jQuery ajax “Uncaught TypeError: Illegal invocation”指jQuery的AJAX报错：未捕获类型错误：非法调用

检查jQuery的文档后发现，如果它不是一个字符串，jQuery的尝试将数据转换成一个字符串。因此，我们需要增加一个选项：processData:false，在这里告诉jQuery不要碰我的数据！另一种选择的contentType:false以防止jQuery来为你添加一个Content-Type头，否则字符串将被丢失和上传失败。最终的ajax代码就像下面这样：
<pre class="prettyprint lang-js"><span class="pln">$</span><span class="pun">.</span><span class="pln">ajax</span><span class="pun">({</span><span class="pln">
    url</span><span class="pun">:</span><span class="pln"> url</span><span class="pun">,</span><span class="pln">
    type</span><span class="pun">:</span> <span class="str">'POST'</span><span class="pun">,</span><span class="pln">
    data</span><span class="pun">:</span><span class="pln"> formdata</span><span class="pun">,</span><span class="pln">
    contentType</span><span class="pun">:</span> <span class="kwd">false</span><span class="pun">,</span> <span class="com">//必须</span><span class="pln">
    processData</span><span class="pun">:</span> <span class="kwd">false</span><span class="pun">,</span> <span class="com">//必须</span><span class="pln">
    dataType</span><span class="pun">:</span> <span class="str">'json'</span><span class="pun">,</span><span class="pln">
    success</span><span class="pun">:</span><span class="pln"> callback
</span><span class="pun">});</span></pre>
或者使用以下
<pre><code>$(function() {
    $('#ifoftheform').ajaxForm(function(result) {
        alert('the form was successfully processed');
    });
});
</code></pre>
&nbsp;
