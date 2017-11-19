---
layout: post
title: PHP cURL https SSL certificate problem: unable to get local issuer certificate 解决方案
date: 2015-03-02 09:31
author: admin
comments: true
categories: []
---
PHP通过cURL访问https时出现SSL certificate problem: unable to get local issuer certificate的解决方法：只要设置以下两个属性就可以解决。
<strong>
将 CURLOPT_SSL_VERIFYPEER 设置为 false,
将 CURLOPT_SSL_VERIFYHOST 设置为 false.
</strong>
代码如下：
<div>
<div id="highlighter_166703" class="syntaxhighlighter  php">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div>
<div class="line number18 index17 alt1">18</div>
<div class="line number19 index18 alt2">19</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="php plain">&lt;?php</code></div>
<div class="line number2 index1 alt1"><code class="php variable">$cURL</code> <code class="php plain">= curl_init();</code></div>
<div class="line number3 index2 alt2"><code class="php variable">$url</code>  <code class="php plain">= </code><code class="php string">'http://www.sh-ow.com/'</code><code class="php plain">;</code></div>
<div class="line number4 index3 alt1"><code class="php plain">curl_setopt_array(</code></div>
<div class="line number5 index4 alt2"><code class="php spaces">        </code><code class="php variable">$cURL</code><code class="php plain">,</code></div>
<div class="line number6 index5 alt1"><code class="php spaces">        </code><code class="php keyword">array</code><code class="php plain">(</code></div>
<div class="line number7 index6 alt2"><code class="php spaces">                </code><code class="php plain">CURLOPT_URL =&gt; </code><code class="php variable">$url</code><code class="php plain">,</code></div>
<div class="line number8 index7 alt1"><code class="php spaces">                </code><code class="php plain">CURLOPT_REFERER =&gt; </code><code class="php variable">$url</code><code class="php plain">,</code></div>
<div class="line number9 index8 alt2"><code class="php spaces">                </code><code class="php plain">CURLOPT_AUTOREFERER =&gt; true,</code></div>
<div class="line number10 index9 alt1"><code class="php spaces">                </code><code class="php plain">CURLOPT_RETURNTRANSFER =&gt; true,</code></div>
<div class="line number11 index10 alt2"><code class="php spaces">                </code><code class="php plain">CURLOPT_SSL_VERIFYPEER =&gt; false,</code></div>
<div class="line number12 index11 alt1"><code class="php spaces">                </code><code class="php plain">CURLOPT_SSL_VERIFYHOST =&gt; false,</code></div>
<div class="line number13 index12 alt2"><code class="php spaces">                </code><code class="php plain">CURLOPT_CONNECTTIMEOUT =&gt; 1,</code></div>
<div class="line number14 index13 alt1"><code class="php spaces">                </code><code class="php plain">CURLOPT_TIMEOUT =&gt; 30,</code></div>
<div class="line number15 index14 alt2"><code class="php spaces">                </code><code class="php plain">CURLOPT_USERAGENT =&gt; </code><code class="php string">'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/34.0.1847.116 Safari/537.36'</code></div>
<div class="line number16 index15 alt1"><code class="php spaces">        </code><code class="php plain">)</code></div>
<div class="line number17 index16 alt2"><code class="php plain">);</code></div>
<div class="line number18 index17 alt1"><code class="php comments">//其他代码...</code></div>
<div class="line number19 index18 alt2"><code class="php plain">?&gt;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
