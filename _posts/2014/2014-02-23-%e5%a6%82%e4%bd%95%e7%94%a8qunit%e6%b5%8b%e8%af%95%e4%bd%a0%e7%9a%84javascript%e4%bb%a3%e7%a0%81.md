---
layout: post
title: 如何用Qunit测试你的JavaScript代码
date: 2014-02-23 15:14
author: admin
comments: true
categories: []
---
<a href="http://docs.jquery.com/QUnit">
QUnit</a>, 由<a title="jquery" href="http://www.woiweb.net/category/jquery" target="_blank">jquery</a>团队开发，是一个对JavasScript进行单元测试的很好的框架。在这篇指南中, 我将具体介绍什么是Qunit，以及为什么你要关心严格地测试你的代码。

&nbsp;
<h2>什么是QUnit</h2>
<a href="http://docs.jquery.com/QUnit">QUnit</a> 是一个非常强大的<a title="javascript" href="http://www.woiweb.net/category/javascript" target="_blank">javascript</a>单元测试框架，可以帮你调试代码。它是由 <a href="http://jquery.com/">jQuery</a> 团队的成员写的，而且是jQuery的官方测试套装。但QUnit一般是足以测试任何常规<a title="javascript" href="http://www.woiweb.net/category/javascript" target="_blank">javascript</a>代码，它甚至可能通过一些<a title="javascript" href="http://www.woiweb.net/category/javascript" target="_blank">javascript</a>引擎比如<a href="http://baike.baidu.com/view/941133.htm#4" target="_blank">Rhino</a>或V8来测试服务器端JavaScript。

如果你不熟悉“单元测试”的概念，请不要担心。这不是很难理解的：
<blockquote>在<a title="计算机编程" href="http://zh.wikipedia.org/wiki/%E8%AE%A1%E7%AE%97%E6%9C%BA%E7%BC%96%E7%A8%8B">计算机编程</a>中，<strong>单元测试</strong>（又称为<strong>模块测试</strong>）是针对<a title="模块 (程序设计)" href="http://zh.wikipedia.org/wiki/%E6%A8%A1%E7%B5%84_%28%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88%29">程序模块</a>(<a title="软件设计" href="http://zh.wikipedia.org/w/index.php?title=%E8%BD%AF%E4%BB%B6%E8%AE%BE%E8%AE%A1&amp;action=edit&amp;redlink=1">软件设计</a>的最小单位)来进行正确性检验的测试工作。程序单元是应用的最小可测试部件。在<a title="过程化编程" href="http://zh.wikipedia.org/w/index.php?title=%E8%BF%87%E7%A8%8B%E5%8C%96%E7%BC%96%E7%A8%8B&amp;action=edit&amp;redlink=1">过程化编程</a>中，一个单元就是单个程序、函数、过程等；对于面向对象编程，最小单元就是方法，包括基类（超类）、抽象类、或者派生类（子类）中的方法。</blockquote>
引自维奇百科。简单地说，你为你的代码的每个功能写测试，如果所有这些测试都通过了，那么你可以肯定的是，代码没有缺陷（通常，还是由你的测试有多彻底而定）。
<h2>为什么你要测试你的代码</h2>
<strong>如果你以前从未写过任何单元测试，你可能直接将你的代码上到网站上，点击一会看看是否有什么问题出现，并且尝试去解决你所发现的问题，采用这种方法会有很多的问题。
</strong>

首先，这是很腻烦的。点击事实上并不是一件轻松的工作，因为你不得不确保每样东西都被点到而且很有可能你错过了一个或两个。

其 次，你为测试做的每件事情是不能复用的，这意味着它很难回归。什么是回归？想像一下你写了一些代码并测试，修复了所有你发现的缺陷，然后发布。此时，一个 用户发送了一些关于新缺陷的反馈，并且需要一些新功能。你返回到代码中，修复这些新缺陷并增加新功能。接下来可能会发生的就是一些旧的缺陷又重现了，这就 叫“回归”。看，现在你还得再去点击一遍，而且有可能你还找不到这些旧的担担缺陷；即使你这么做，这还需要一段时间才能弄清楚你的问题是由回归引起的。使 用单元测试，你写测试去发现缺陷，一旦代码被修改，您通过测试再筛选一次。如果回归出现，一些测试一定会失败，你可以很容易地认出他们，知道哪部分代码包 含了错误。既然你知道你刚才修改了什么，就可以很容易地解决。

另外一个单元测试的优点，尤其是对于web开发来说: 它使跨浏览器兼容性测试很容易。仅仅在不同浏览器中运行你的测试案例就行，如果一个浏览器出现问题，你修复它并重新运行这些测试案例，确保不会在别的浏览 器引起回归，一旦全部通过测试，你可以肯定的说，所有的目标浏览器都支持。

我想提及一个John Resig的项目：<a href="http://testswarm.com/">TestSwarm</a>。 它将Javascript单元测试带到了一个新的层次，通过使其分布，这是一个网站，其中包含很多测试案例，任何人都可以去那运行一些测试案例，然后返回结果会返回到服务器。通过这种方式，代码会非常迅速的在不同的浏览器进行测试，甚至不同的平台运行。
<h2>如何用QUnit写单元测试</h2>
那么，你如何正确地用QUnit写单元测试呢？首先，您需要设置一个测试环境：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>&lt;!DOCTYPE html&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>&lt;</code><code>html</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td><code>&lt;</code><code>head</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td><code>    </code><code>&lt;</code><code>title</code><code>&gt;QUnit Test Suite&lt;/</code><code>title</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td><code>    </code><code>&lt;</code><code>link</code> <code>rel</code><code>=</code><code>"stylesheet"</code> <code>href</code><code>=</code><code>"<a href="http://github.com/jquery/qunit/raw/master/qunit/qunit.css">http://github.com/jquery/qunit/raw/master/qunit/qunit.css</a>"</code><code>type</code><code>=</code><code>"text/css"</code> <code>media</code><code>=</code><code>"screen"</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td><code>    </code><code>&lt;</code><code>script</code> <code>type</code><code>=</code><code>"text/javascript"</code><code>src</code><code>=</code><code>"<a href="http://github.com/jquery/qunit/raw/master/qunit/qunit.js">http://github.com/jquery/qunit/raw/master/qunit/qunit.js</a>"</code><code>&gt;&lt;/</code><code>script</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td><code>    </code><code>&lt;!-- Your project file goes here --&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td><code>    </code><code>&lt;</code><code>script</code> <code>type</code><code>=</code><code>"text/javascript"</code> <code>src</code><code>=</code><code>"myProject.js"</code><code>&gt;&lt;/</code><code>script</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>    </code><code>&lt;!-- Your tests file goes here --&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>    </code><code>&lt;</code><code>script</code> <code>type</code><code>=</code><code>"text/javascript"</code> <code>src</code><code>=</code><code>"myTests.js"</code><code>&gt;&lt;/</code><code>script</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>&lt;/</code><code>head</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>12</code></td>
<td><code>&lt;</code><code>body</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>13</code></td>
<td><code>    </code><code>&lt;</code><code>h1</code> <code>id</code><code>=</code><code>"qunit-header"</code><code>&gt;QUnit Test Suite&lt;/</code><code>h1</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>14</code></td>
<td><code>    </code><code>&lt;</code><code>h2</code> <code>id</code><code>=</code><code>"qunit-banner"</code><code>&gt;&lt;/</code><code>h2</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>15</code></td>
<td><code>    </code><code>&lt;</code><code>div</code> <code>id</code><code>=</code><code>"qunit-testrunner-toolbar"</code><code>&gt;&lt;/</code><code>div</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>16</code></td>
<td><code>    </code><code>&lt;</code><code>h2</code> <code>id</code><code>=</code><code>"qunit-userAgent"</code><code>&gt;&lt;/</code><code>h2</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>17</code></td>
<td><code>    </code><code>&lt;</code><code>ol</code> <code>id</code><code>=</code><code>"qunit-tests"</code><code>&gt;&lt;/</code><code>ol</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>18</code></td>
<td><code>&lt;/</code><code>body</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>19</code></td>
<td><code>&lt;/</code><code>html</code><code>&gt;</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
正如你所见，在这里使用了一个<a href="http://github.com/jquery/qunit/raw/master/qunit/qunit.js">被托管的QUnit框架版本</a>。

将要被测试的代码已被添加到myProject.js中，而且你的测试应该插入到myTest.js。要运行这些测试，只需在一个浏览器中打开这个<a title="html" href="http://www.woiweb.net/category/html_css" target="_blank">html</a>文件。现在到了写些测试的时间了。

单元测试的基石是断主。
<blockquote>断言是一个命题，预测你的代码的返回结果。如果预测是假的，断言失败，你就知道出了问题。</blockquote>
运行断言，你应该把它们放入测试案例：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>// Let's test this function</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>function</code> <code>isEven(val) {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td><code>    </code><code>return</code> <code>val % 2 === 0;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td><code>test(</code><code>'isEven()'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td><code>    </code><code>ok(isEven(0), </code><code>'Zero is an even number'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td><code>    </code><code>ok(isEven(2), </code><code>'So is two'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>    </code><code>ok(isEven(-4), </code><code>'So is negative four'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>    </code><code>ok(!isEven(1), </code><code>'One is not an even number'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>    </code><code>ok(!isEven(-7), </code><code>'Neither is negative seven'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>12</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
这里我们定义一个函数：isEven，用来检测一个数字是否为奇数，并且我们希望测试这个函数来确认它不会返回错误答案。

我们首先调用test()，它构建了一个测试案例；第一个参数是一个将被显示在结果中的字符串，第二个参数是包括我们断主的一个回调函数。

我们写了5个断言，所有的都是布尔型的。一个布尔型的断言，期望它的第一个参数为true。第二个参数依然是要显示在结果中的消息。

这里是你想要得到的，只要你运行测试：
<div><img alt="a test for isEven()" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/iseven_test.png" /></div>
由于所有的断言都已成功通过，我们可以高兴的认为<em>isEven()工作正常。</em>

让我们看看如果一个断言失败了会发生什么。
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>// Let's test this function</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>function</code> <code>isEven(val) {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td><code>    </code><code>return</code> <code>val % 2 === 0;</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td><code>test(</code><code>'isEven()'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td><code>    </code><code>ok(isEven(0), </code><code>'Zero is an even number'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td><code>    </code><code>ok(isEven(2), </code><code>'So is two'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>    </code><code>ok(isEven(-4), </code><code>'So is negative four'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>    </code><code>ok(!isEven(1), </code><code>'One is not an even number'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>    </code><code>ok(!isEven(-7), </code><code>'Neither does negative seven'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>12</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>13</code></td>
<td><code>    </code><code>// Fails</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>14</code></td>
<td><code>    </code><code>ok(isEven(3), </code><code>'Three is an even number'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>15</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
这是结果：
<div><img alt="a test contains failed assertion for isEven()" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/iseven_test_fail.png" /></div>
该断言失败因为我们故意把它写错，但是在你的项目中，如果测试未通过，并且所有的断言都是正确的，你将发现一个bug。
<h2>更多断言</h2>
ok()不仅是QUnit提供的唯一断言， 当在测试你的项目时，还会有一些非常有用的其他类型的断言:
<h2>比较断言</h2>
比较断言，equals()，期望它的第一个参数（是实际值）等于它的第二个参数（期望值）。它很类似于ok()，但均会输入实现和期望值，使得高度更加简单，像ok()一样，它可带一个可选的第三个参数作为显示的消息。

所以可以代替：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>1</code></td>
<td><code>test(</code><code>'assertions'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>2</code></td>
<td><code>    </code><code>ok( 1 == 1, </code><code>'one equals one'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>3</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<div><img alt="a boolean assertion" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/boolean_assertion.png" /></div>
你可以这样写：

&nbsp;
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>1</code></td>
<td><code>test(</code><code>'assertions'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>2</code></td>
<td><code>    </code><code>equals( 1, 1, </code><code>'one equals one'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>3</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<div><img alt="a comparison assertion" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/comparison_assertion.png" /></div>
注意最后一个“1”，这是比较值

如果两个值不相等：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>1</code></td>
<td><code>test(</code><code>'assertions'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>2</code></td>
<td><code>    </code><code>equals( 2, 1, </code><code>'one equals one'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>3</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<div><img alt="a failed comparison assertion" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/comparison_assertion_fail.png" /></div>
提供更多些信息，让生活更简单些。

比较断言使用“==”来比较它的参数，所以它不能处理数组或对象的比较：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>1</code></td>
<td><code>test(</code><code>'test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>2</code></td>
<td><code>    </code><code>equals( {}, {}, </code><code>'fails, these are different objects'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>3</code></td>
<td><code>    </code><code>equals( {a: 1}, {a: 1} , </code><code>'fails'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>4</code></td>
<td><code>    </code><code>equals( [], [], </code><code>'fails, there are different arrays'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>5</code></td>
<td><code>    </code><code>equals( [1], [1], </code><code>'fails'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>6</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
为了测试这种相等，QUnit提供了另外一种断言：<strong>恒等断言</strong>。
<h2>恒等断言</h2>
恒等断言，same()，期望相同的参数相等，但是它较深的采用递归比较断言，不仅作用于原始类型，而且包括数组和对象。断言，在前面的例子中，如果你把他们改成恒等断言将全部通过。
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>1</code></td>
<td><code>test(</code><code>'test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>2</code></td>
<td><code>    </code><code>same( {}, {}, </code><code>'passes, objects have the same content'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>3</code></td>
<td><code>    </code><code>same( {a: 1}, {a: 1} , </code><code>'passes'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>4</code></td>
<td><code>    </code><code>same( [], [], </code><code>'passes, arrays have the same content'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>5</code></td>
<td><code>    </code><code>same( [1], [1], </code><code>'passes'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>6</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
注意same()使用”===”去比较，如有必要的话，所以它在比较特殊值的时候就派上用场了。
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>1</code></td>
<td><code>test(</code><code>'test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>2</code></td>
<td><code>    </code><code>equals( 0, </code><code>false</code><code>, </code><code>'true'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>3</code></td>
<td><code>    </code><code>same( 0, </code><code>false</code><code>, </code><code>'false'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>4</code></td>
<td><code>    </code><code>equals( </code><code>null</code><code>, undefined, </code><code>'true'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>5</code></td>
<td><code>    </code><code>same( </code><code>null</code><code>, undefined, </code><code>'false'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>6</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<h2>结构化你的断言</h2>
把所有的断言放在一个单独的测试案例中是相当不好的想法，因为这很难去维护，并且不能返回一个纯净的结果。你需要做的就是结构化他们，把他们放在不同的测试案例，每个目标为一个单独功能。

你可以甚至通过调用模块函数来把测试案例组织到不同的模块：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>1</code></td>
<td><code>module(</code><code>'Module A'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>2</code></td>
<td><code>test(</code><code>'a test'</code><code>, </code><code>function</code><code>() {});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>3</code></td>
<td><code>test(</code><code>'an another test'</code><code>, </code><code>function</code><code>() {});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>4</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>5</code></td>
<td><code>module(</code><code>'Module B'</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>6</code></td>
<td><code>test(</code><code>'a test'</code><code>, </code><code>function</code><code>() {});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>7</code></td>
<td><code>test(</code><code>'an another test'</code><code>, </code><code>function</code><code>() {});</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<div><img alt="structure assertions" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/structure_assertions.png" /></div>
<h2>异步测试</h2>
在前面的示例中，所有的断言都是同步调用的，这意味着他们是一个接着一个运行的。在这个真实的世界，同样 存在着很多异步的函数，例如<a title="Ajax" href="http://www.woiweb.net/tag/ajax" target="_blank">Ajax</a>请求或通过setTimeout()或sestInterval()调用的方法。我们如何去测试这些种类的方法呢？QUnit提供了一个特殊的叫做和“异步测试”的测试案例，提供给异步的测试：

让我们首先尝试用常规的方法写：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>1</code></td>
<td><code>test(</code><code>'asynchronous test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>2</code></td>
<td><code>    </code><code>setTimeout(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>3</code></td>
<td><code>        </code><code>ok(</code><code>true</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>4</code></td>
<td><code>    </code><code>}, 100)</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>5</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<div><img alt="an incorrent example of asychronous test" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/async_test_wrong.png" /></div>
看？这就好像我们没有写任何断言一样。这是因为断言是被异步执行的，到它被调用的时候，此次测试已经执行完成。

这是正确的版本：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>test(</code><code>'asynchronous test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>    </code><code>// Pause the test first</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td><code>    </code><code>stop();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td><code>    </code><code>setTimeout(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td><code>        </code><code>ok(</code><code>true</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td><code>        </code><code>// After the assertion has been called,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>        </code><code>// continue the test</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>        </code><code>start();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>    </code><code>}, 100)</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>12</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<div><img alt="a correct example of asychronous test" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/async_test.png" /></div>
在这，我们使用了stop()去暂停此次测试案例， 并且在断言被调用以后，我们使用start()继续。

在调用完test()后立即调用stop()是很平常的；所以QUnit提供了一个捷径：asyncTest()。你可以像这样重写之前的示例：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>asyncTest(</code><code>'asynchronous test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>    </code><code>// The test is automatically paused</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td><code>    </code><code>setTimeout(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td><code>        </code><code>ok(</code><code>true</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td><code>        </code><code>// After the assertion has been called,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td><code>        </code><code>// continue the test</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>        </code><code>start();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>    </code><code>}, 100)</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
还有一点要注意：setTimeout()通常会调用它自己的回调函数，但如果它是一个自定义的函数（例如：一个<a title="Ajax" href="http://www.woiweb.net/tag/ajax" target="_blank">Ajax</a>调用）。你如何确认回调函数被调用了呢？并且如果回调函数没有被调用，start()将不会被执行，整个单元测试将被挂起：
<div><img alt="unit testing hangs" src="http://d2o0t5hpnwv4c1.cloudfront.net/562_qunit/unit_testing_hangs.png" /></div>
所以这就是你需要做的：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>// A custom function</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>function</code> <code>ajax(successCallback) {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td><code>    </code><code>$.ajax({</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td><code>        </code><code>url: </code><code>'server.php'</code><code>,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td><code>        </code><code>success: successCallback</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td><code>    </code><code>});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>test(</code><code>'asynchronous test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>    </code><code>// Pause the test, and fail it if start() isn't called after one second</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>    </code><code>stop(1000);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>12</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>13</code></td>
<td><code>    </code><code>ajax(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>14</code></td>
<td><code>        </code><code>// ...asynchronous assertions</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>15</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>16</code></td>
<td><code>        </code><code>start();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>17</code></td>
<td><code>    </code><code>})</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>18</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
你可以通过延时去stop()，它告知QUnit，“如果start()在延时后没有被调用，你应未通过测试”。你可以确认的是整个测试没有挂起而且如果哪里出了问题你可以注意到。

那么多个异步函数呢？你在哪里放置start()？可把它放在setTimeout()里：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>// A custom function</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>function</code> <code>ajax(successCallback) {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td><code>    </code><code>$.ajax({</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td><code>        </code><code>url: </code><code>'server.php'</code><code>,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td><code>        </code><code>success: successCallback</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td><code>    </code><code>});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>test(</code><code>'asynchronous test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>    </code><code>// Pause the test</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>    </code><code>stop();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>12</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>13</code></td>
<td><code>    </code><code>ajax(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>14</code></td>
<td><code>        </code><code>// ...asynchronous assertions</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>15</code></td>
<td><code>    </code><code>})</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>16</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>17</code></td>
<td><code>    </code><code>ajax(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>18</code></td>
<td><code>        </code><code>// ...asynchronous assertions</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>19</code></td>
<td><code>    </code><code>})</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>20</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>21</code></td>
<td><code>    </code><code>setTimeout(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>22</code></td>
<td><code>        </code><code>start();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>23</code></td>
<td><code>    </code><code>}, 2000);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>24</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
延时应该适当的长足够来允许二者的回调函数在测试继续执行前被调用。但是如果其中一个回调函数没有被调用怎么办？你怎样去知道？这就是expect()加入的原因：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>// A custom function</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>function</code> <code>ajax(successCallback) {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td><code>    </code><code>$.ajax({</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td><code>        </code><code>url: </code><code>'server.php'</code><code>,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td><code>        </code><code>success: successCallback</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td><code>    </code><code>});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>test(</code><code>'asynchronous test'</code><code>, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>    </code><code>// Pause the test</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>    </code><code>stop();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>12</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>13</code></td>
<td><code>    </code><code>// Tell QUnit that you expect three assertions to run</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>14</code></td>
<td><code>    </code><code>expect(3);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>15</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>16</code></td>
<td><code>    </code><code>ajax(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>17</code></td>
<td><code>        </code><code>ok(</code><code>true</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>18</code></td>
<td><code>    </code><code>})</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>19</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>20</code></td>
<td><code>    </code><code>ajax(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>21</code></td>
<td><code>        </code><code>ok(</code><code>true</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>22</code></td>
<td><code>        </code><code>ok(</code><code>true</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>23</code></td>
<td><code>    </code><code>})</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>24</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>25</code></td>
<td><code>    </code><code>setTimeout(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>26</code></td>
<td><code>        </code><code>start();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>27</code></td>
<td><code>    </code><code>}, 2000);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>28</code></td>
<td><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
你给expect()传一个数字告知QUnit你期望X个断言去执行，如果一个断言未被执行，这个数字将不会匹配，而且你瘵会注意到有些东西出错了。

这仍有一个expect()的捷径：你只需给test()或asyncTest()的第二个参数传递一个数字：
<div>
<div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>01</code></td>
<td><code>// A custom function</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>02</code></td>
<td><code>function</code> <code>ajax(successCallback) {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>03</code></td>
<td><code>    </code><code>$.ajax({</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>04</code></td>
<td><code>        </code><code>url: </code><code>'server.php'</code><code>,</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>05</code></td>
<td><code>        </code><code>success: successCallback</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>06</code></td>
<td><code>    </code><code>});</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>07</code></td>
<td><code>}</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>08</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>09</code></td>
<td><code>// Tell QUnit that you expect three assertion to run</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>10</code></td>
<td><code>test(</code><code>'asynchronous test'</code><code>, 3, </code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>11</code></td>
<td><code>    </code><code>// Pause the test</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>12</code></td>
<td><code>    </code><code>stop();</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>13</code></td>
<td></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>14</code></td>
<td><code>    </code><code>ajax(</code><code>function</code><code>() {</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>15</code></td>
<td><code>        </code><code>ok(</code><code>true</code><code>);</code></td>
</tr>
</tbody>
</table>
</div>
<div>
<table border="0">
<tbody>
<tr>
<td><code>16</code></td>
<td><code>    </code><code>})</code></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
