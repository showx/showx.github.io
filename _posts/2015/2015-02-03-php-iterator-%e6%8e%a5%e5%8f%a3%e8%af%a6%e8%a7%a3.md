---
layout: post
title: php Iterator 接口详解
date: 2015-02-03 14:39
author: admin
comments: true
categories: []
---
大家都知道，数组是可以使用foreach 循环的，我们也可以把一个对象当做数组，做循环操作。

1、对象继承 Iterator 接口
<div class="dp-highlighter bg_php">
<div class="bar">
<div class="tools"><b>[php]</b> <a class="ViewSource" title="view plain" href="http://blog.csdn.net/wlm131127/article/details/14161809#">view plain</a><a class="CopyToClipboard" title="copy" href="http://blog.csdn.net/wlm131127/article/details/14161809#">copy</a>
<div><embed id="ZeroClipboardMovie_1" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_1"></embed></div>
</div>
</div>
<ol class="dp-c" start="1">
	<li class="alt">&lt;?php</li>
	<li class="">        <span class="keyword">class</span> MyIterator <span class="keyword">implements</span> Iterator{</li>
	<li class="alt">            <span class="keyword">private</span> <span class="vars">$_d</span> = <span class="keyword">array</span>(<span class="string">'a'</span>,<span class="string">'b'</span>,<span class="string">'c'</span>,<span class="string">'d'</span>);</li>
	<li class="">            <span class="keyword">private</span> <span class="vars">$_p</span> = 0;</li>
	<li class="alt">            <span class="keyword">public</span> <span class="keyword">function</span> __construct(){</li>
	<li class="">                <span class="vars">$this</span>-&gt;_p = 0;</li>
	<li class="alt">            }</li>
	<li class="">            <span class="comment">/**</span></li>
	<li class="alt"><span class="comment">             * 返回当前值</span></li>
	<li class=""><span class="comment">             * </span></li>
	<li class="alt"><span class="comment">             * @see Iterator::current()</span></li>
	<li class=""><span class="comment">             */</span></li>
	<li class="alt">            <span class="keyword">public</span> <span class="keyword">function</span> current() {</li>
	<li class="">                var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="alt">                <span class="keyword">return</span> <span class="vars">$this</span>-&gt;_d[<span class="vars">$this</span>-&gt;_p];</li>
	<li class=""></li>
	<li class="alt">            }</li>
	<li class=""></li>
	<li class="alt">            <span class="comment">/**</span></li>
	<li class=""><span class="comment">             * 当前索引加1</span></li>
	<li class="alt"><span class="comment">             * </span></li>
	<li class=""><span class="comment">             * </span></li>
	<li class="alt"><span class="comment">             * @see Iterator::next()</span></li>
	<li class=""><span class="comment">             */</span></li>
	<li class="alt">            <span class="keyword">public</span> <span class="keyword">function</span> next() {</li>
	<li class="">                var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="alt">                <span class="vars">$this</span>-&gt;_p++;</li>
	<li class="">            }</li>
	<li class="alt">            <span class="comment">/**</span></li>
	<li class=""><span class="comment">             * 返回当前索引值</span></li>
	<li class="alt"><span class="comment">             * </span></li>
	<li class=""><span class="comment">             * </span></li>
	<li class="alt"><span class="comment">             * @see Iterator::key()</span></li>
	<li class=""><span class="comment">             */</span></li>
	<li class="alt">            <span class="keyword">public</span> <span class="keyword">function</span> key() {</li>
	<li class="">                var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="alt">                <span class="keyword">return</span> <span class="vars">$this</span>-&gt;_p;</li>
	<li class=""></li>
	<li class="alt">            }</li>
	<li class="">            <span class="comment">/**</span></li>
	<li class="alt"><span class="comment">             * 对当前值进行验证</span></li>
	<li class=""><span class="comment">             * </span></li>
	<li class="alt"><span class="comment">             * @see Iterator::valid()</span></li>
	<li class=""><span class="comment">             */</span></li>
	<li class="alt">            <span class="keyword">public</span> <span class="keyword">function</span> valid() {</li>
	<li class="">                var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="alt">                <span class="keyword">return</span> isset(<span class="vars">$this</span>-&gt;_d[<span class="vars">$this</span>-&gt;_p]);</li>
	<li class=""></li>
	<li class="alt">            }</li>
	<li class="">            <span class="comment">/**</span></li>
	<li class="alt"><span class="comment">             * 将数组索引置为0</span></li>
	<li class=""><span class="comment">             * </span></li>
	<li class="alt"><span class="comment">             * </span></li>
	<li class=""><span class="comment">             * @see Iterator::rewind()</span></li>
	<li class="alt"><span class="comment">             */</span></li>
	<li class="">            <span class="keyword">public</span> <span class="keyword">function</span> <span class="func">rewind</span>() {</li>
	<li class="alt">                var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="">                <span class="vars">$this</span>-&gt;_p = 0;</li>
	<li class="alt">            }</li>
	<li class=""></li>
	<li class="alt">        }</li>
	<li class="">    <span class="vars">$me</span> = <span class="keyword">new</span> MyIterator();</li>
	<li class="alt">    <span class="keyword">foreach</span>(<span class="vars">$me</span> <span class="keyword">as</span> <span class="vars">$key</span> =&gt; <span class="vars">$value</span>){</li>
	<li class="">        var_dump(<span class="vars">$key</span>) . <span class="string">':'</span> . var_dump(<span class="vars">$value</span>);</li>
	<li class="alt">    }</li>
	<li class=""> 结果如下图：</li>
</ol>
</div>
<img src="http://img.blog.csdn.net/20131105100714421?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2xtMTMxMTI3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="" width="254" height="516" />

让我们分析一下：

1、将对象中数组的索引置为0.

2、验证该索引下是否有值。

3、如果有、返回值。

4、返回索引值。

5、调用next函数，索引值加1.

6、重复2、3、4、步奏。如果验证某索引下没有值，就此中断。

缺点：1、我们需要实现这么多的函数(5个。

2、上面说到，如果某一索引下没有值，则就此中断了，不在循环，如果我们的数组是这样的private $_d = array('a','b','c','d',8 =&gt; 'f');。那么f值就不会被输出。

我们先来解决第一个缺点。

不实现接口Iterator，实现他的子接口<span class="ooclass"><strong class="classname">IteratorAggregate</strong></span>
<div class="dp-highlighter bg_css">
<div class="bar">
<div class="tools"><b>[css]</b> <a class="ViewSource" title="view plain" href="http://blog.csdn.net/wlm131127/article/details/14161809#">view plain</a><a class="CopyToClipboard" title="copy" href="http://blog.csdn.net/wlm131127/article/details/14161809#">copy</a>
<div><embed id="ZeroClipboardMovie_2" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_2"></embed></div>
</div>
</div>
<ol class="dp-css" start="1">
	<li class="alt">class myData implements IteratorAggregate {</li>
	<li class="">            public $property<span class="value">1</span> = <span class="string">"Public property one"</span>;</li>
	<li class="alt">            public $property<span class="value">2</span> = <span class="string">"Public property two"</span>;</li>
	<li class="">            public $property<span class="value">3</span> = <span class="string">"Public property three"</span>;</li>
	<li class="alt"></li>
	<li class="">            public function __construct() {</li>
	<li class="alt">                $this-&gt;property<span class="value">4</span> = <span class="string">"last property"</span>;</li>
	<li class="">            }</li>
	<li class="alt"></li>
	<li class="">            public function getIterator() {</li>
	<li class="alt">                return new ArrayIterator($this);</li>
	<li class="">            }</li>
	<li class="alt">        }</li>
	<li class=""></li>
	<li class="alt">        $obj = new myData;</li>
	<li class=""></li>
	<li class="alt">        foreach($obj as $key =&gt; $value) {</li>
	<li class="">            var_dump($key, $value);</li>
	<li class="alt">            echo <span class="string">"\n"</span>;</li>
	<li class="">        }</li>
</ol>
</div>
输出如下图：

<img src="http://img.blog.csdn.net/20131105102303562?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2xtMTMxMTI3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="" />

我们只需实现一个方法getIterator就OK了。是不是简单了。我们也可以直接实现MyIterator ，看看出现什么样的结果，自己去尝试下吧。

解决第二个缺点，这就需要我们来完善MyIterator，保证当索引不连续时，也能输出后面的值。封装的思想啊，大家尝试下吧。

下面再说一说Traversable接口，这个接口中什么样的抽样方法都没有，他是<a href="http://www.php.net/manual/en/class.iterator.php" target="_blank">Iterator</a>的父接口。最底层的，这个接口一般不会去实现它，如果非得要实现它，记住两点。

1、必须和<a href="http://www.php.net/manual/en/class.iterator.php" target="_blank">Iterator</a>、IteratorAggregate两个接口中的某一个或者两个一起用。

2、IteratorAggregate或者<a href="http://www.php.net/manual/en/class.iterator.php" target="_blank">Iterator</a>必须放在Traversable前面。如：class MyIterator implements Iterator,Traversable{}。切记。

上面我们说的都是与循环相关的接口，下面我们就来说一说与赋值、销毁值等相关的接口，那就是ArrayAccess 。

首先，一段代码，实现以下该接口
<div class="dp-highlighter bg_php">
<div class="bar">
<div class="tools"><b>[php]</b> <a class="ViewSource" title="view plain" href="http://blog.csdn.net/wlm131127/article/details/14161809#">view plain</a><a class="CopyToClipboard" title="copy" href="http://blog.csdn.net/wlm131127/article/details/14161809#">copy</a>
<div><embed id="ZeroClipboardMovie_3" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_3"></embed></div>
</div>
</div>
<ol class="dp-c" start="1">
	<li class="alt">&lt;?php</li>
	<li class="">        <span class="keyword">class</span> MyArray <span class="keyword">implements</span> ArrayAccess{</li>
	<li class="alt"></li>
	<li class="">        <span class="keyword">private</span> <span class="vars">$_d</span> = <span class="keyword">array</span>(<span class="string">'a'</span>,<span class="string">'v'</span>,<span class="string">'b'</span>,<span class="string">'d'</span>);</li>
	<li class="alt"></li>
	<li class="">        <span class="comment">/**</span></li>
	<li class="alt"><span class="comment">         *  检测某个索引下的值是否存在</span></li>
	<li class=""><span class="comment">         * </span></li>
	<li class="alt"><span class="comment">         * @see ArrayAccess::offsetExists()</span></li>
	<li class=""><span class="comment">         */</span></li>
	<li class="alt">        <span class="keyword">public</span> <span class="keyword">function</span> offsetExists(<span class="vars">$offset</span>) {</li>
	<li class="">            var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="alt">            <span class="keyword">return</span> <span class="vars">$this</span>-&gt;_d[<span class="vars">$offset</span>] !== null;</li>
	<li class=""></li>
	<li class="alt">        }</li>
	<li class=""></li>
	<li class="alt">        <span class="comment">/**</span></li>
	<li class=""><span class="comment">         * 取得某一索引上的值</span></li>
	<li class="alt"><span class="comment">         * </span></li>
	<li class=""><span class="comment">         * @see ArrayAccess::offsetGet()</span></li>
	<li class="alt"><span class="comment">         */</span></li>
	<li class="">        <span class="keyword">public</span> <span class="keyword">function</span> offsetGet(<span class="vars">$offset</span>) {</li>
	<li class="alt">            var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="">            <span class="keyword">return</span> <span class="vars">$this</span>-&gt;_d[<span class="vars">$offset</span>];</li>
	<li class="alt"></li>
	<li class="">        }</li>
	<li class="alt"></li>
	<li class="">        <span class="comment">/**</span></li>
	<li class="alt"><span class="comment">         * 设置某一索引上的值</span></li>
	<li class=""><span class="comment">         * </span></li>
	<li class="alt"><span class="comment">         * @see ArrayAccess::offsetSet()</span></li>
	<li class=""><span class="comment">         */</span></li>
	<li class="alt">        <span class="keyword">public</span> <span class="keyword">function</span> offsetSet(<span class="vars">$offset</span>, <span class="vars">$value</span>) {</li>
	<li class="">            var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="alt">            <span class="vars">$this</span>-&gt;_d[<span class="vars">$offset</span>] = <span class="vars">$value</span>;</li>
	<li class=""></li>
	<li class="alt">        }</li>
	<li class=""></li>
	<li class="alt">        <span class="comment">/**</span></li>
	<li class=""><span class="comment">         * 销毁某一索引处的值</span></li>
	<li class="alt"><span class="comment">         * </span></li>
	<li class=""><span class="comment">         * </span></li>
	<li class="alt"><span class="comment">         * @see ArrayAccess::offsetUnset()</span></li>
	<li class=""><span class="comment">         */</span></li>
	<li class="alt">        <span class="keyword">public</span> <span class="keyword">function</span> offsetUnset(<span class="vars">$offset</span>) {</li>
	<li class="">            var_dump(<span class="keyword">__METHOD__</span>);</li>
	<li class="alt">            <span class="vars">$this</span>-&gt;_d[<span class="vars">$offset</span>] = null;</li>
	<li class="">        }</li>
	<li class="alt"></li>
	<li class="">}</li>
	<li class="alt">        <span class="vars">$me</span> = <span class="keyword">new</span> MyArray();</li>
	<li class="">        <span class="func">echo</span> <span class="vars">$me</span>[0];</li>
	<li class="alt">        <span class="func">echo</span> <span class="string">"&lt;br /&gt;"</span>;</li>
	<li class="">        <span class="vars">$me</span>[5] = <span class="string">'t'</span> ;</li>
	<li class="alt">        <span class="func">echo</span> <span class="string">"&lt;br /&gt;"</span>;</li>
	<li class="">        <span class="func">empty</span>(<span class="vars">$me</span>[1]) ;</li>
	<li class="alt">        <span class="func">echo</span> <span class="string">"&lt;br /&gt;"</span>;</li>
	<li class="">        unset(<span class="vars">$me</span>[2]) ;</li>
	<li class="alt"></li>
</ol>
</div>
结果如图：

<img src="http://img.blog.csdn.net/20131105104601265?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2xtMTMxMTI3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast" alt="" />

一个小问题，留给大家：

如果我们要测试empty($me[10]);会出现什么样的结果呢，大家试下吧。

实现这几个接口完全是为了封装，使其更好的为我们服务。

最后，仅是个人想法，大家共同交流学习。谢谢
