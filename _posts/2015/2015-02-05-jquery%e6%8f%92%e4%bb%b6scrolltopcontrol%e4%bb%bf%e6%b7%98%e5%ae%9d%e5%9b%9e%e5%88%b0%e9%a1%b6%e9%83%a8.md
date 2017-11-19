---
layout: post
title: jquery插件scrolltopcontrol仿淘宝回到顶部
date: 2015-02-05 10:05
author: admin
comments: true
categories: []
---
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>001</code></td>
<td class="content"><code class="plain">&lt;script type=</code><code class="string">"text/javascript"</code> <code class="plain">src=</code><code class="string">"https://ajax.googleapis.com/ajax/libs/jquery/1.6.4/jquery.min.js"</code><code class="plain">&gt;&lt;/script&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>002</code></td>
<td class="content"><code class="plain">&lt;html&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>003</code></td>
<td class="content"><code class="plain">&lt;head&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>004</code></td>
<td class="content"><code class="plain">&lt;title&gt;仿淘宝回到顶部jquery插件&lt;/title&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>005</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>006</code></td>
<td class="content"><code class="plain">&lt;script type=</code><code class="string">"text/javascript"</code> <code class="plain">&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>007</code></td>
<td class="content"><code class="comments">//** jQuery Scroll to Top Control script- (c) Dynamic Drive DHTML code library: http://www.dynamicdrive.com.</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>008</code></td>
<td class="content"><code class="comments">//** Available/ usage terms at http://www.dynamicdrive.com (March 30th, 09')</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>009</code></td>
<td class="content"><code class="comments">//** v1.1 (April 7th, 09'):</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>010</code></td>
<td class="content"><code class="comments">//** 1) Adds ability to scroll to an absolute position (from top of page) or specific element on the page instead.</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>011</code></td>
<td class="content"><code class="comments">//** 2) Fixes scroll animation not working in Opera.</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>012</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>013</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>014</code></td>
<td class="content"><code class="keyword">var</code> <code class="plain">scrolltotop={</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>015</code></td>
<td class="content"><code class="spaces"> </code><code class="comments">//startline: Integer. Number of pixels from top of doc scrollbar is scrolled before showing control</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>016</code></td>
<td class="content"><code class="spaces"> </code><code class="comments">//scrollto: Keyword (Integer, or "Scroll_to_Element_ID"). How far to scroll document up when control is clicked on (0=top).</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>017</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">setting: {startline:1, scrollto: 0, scrollduration:1000, fadeduration:[500, 100]},</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>018</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">controlHTML: </code><code class="string">'&lt;img src="http://www.yzzmf.com/images/demo/gotop.gif" style="width:31px; height:31px" /&gt;'</code><code class="plain">, //HTML </code><code class="keyword">for</code> <code class="plain">control, which is auto wrapped </code><code class="keyword">in</code> <code class="plain">DIV w/ ID=</code><code class="string">"topcontrol"</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>019</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">controlattrs: {offsetx:100, offsety:80}, </code><code class="comments">//offset of control relative to right/ bottom of window corner</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>020</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">anchorkeyword: </code><code class="string">'#top'</code><code class="plain">, </code><code class="comments">//Enter href value of HTML anchors on the page that should also act as "Scroll Up" links</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>021</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>022</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">state: {isvisible:</code><code class="keyword">false</code><code class="plain">, shouldvisible:</code><code class="keyword">false</code><code class="plain">},</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>023</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>024</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">scrollup:</code><code class="keyword">function</code><code class="plain">(){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>025</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">if</code> <code class="plain">(!</code><code class="keyword">this</code><code class="plain">.cssfixedsupport) </code><code class="comments">//if control is positioned using JavaScript</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>026</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">this</code><code class="plain">.$control.css({opacity:0}) </code><code class="comments">//hide control immediately after clicking it</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>027</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">var</code> <code class="plain">dest=isNaN(</code><code class="keyword">this</code><code class="plain">.setting.scrollto)? </code><code class="keyword">this</code><code class="plain">.setting.scrollto : parseInt(</code><code class="keyword">this</code><code class="plain">.setting.scrollto)</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>028</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">if</code> <code class="plain">(</code><code class="keyword">typeof</code> <code class="plain">dest==</code><code class="string">"string"</code> <code class="plain">&amp;&amp; jQuery(</code><code class="string">'#'</code><code class="plain">+dest).length==1) </code><code class="comments">//check element set by string exists</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>029</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">dest=jQuery(</code><code class="string">'#'</code><code class="plain">+dest).offset().top</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>030</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">else</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>031</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">dest=0</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>032</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">this</code><code class="plain">.$body.animate({scrollTop: dest}, </code><code class="keyword">this</code><code class="plain">.setting.scrollduration);</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>033</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">},</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>034</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>035</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">keepfixed:</code><code class="keyword">function</code><code class="plain">(){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>036</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">var</code> <code class="plain">$window=jQuery(window)</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>037</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">var</code> <code class="plain">controlx=$window.scrollLeft() + $window.width() - </code><code class="keyword">this</code><code class="plain">.$control.width() - </code><code class="keyword">this</code><code class="plain">.controlattrs.offsetx</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>038</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">var</code> <code class="plain">controly=$window.scrollTop() + $window.height() - </code><code class="keyword">this</code><code class="plain">.$control.height() - </code><code class="keyword">this</code><code class="plain">.controlattrs.offsety</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>039</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">this</code><code class="plain">.$control.css({left:controlx+</code><code class="string">'px'</code><code class="plain">, top:controly+</code><code class="string">'px'</code><code class="plain">})</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>040</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">},</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>041</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>042</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">togglecontrol:</code><code class="keyword">function</code><code class="plain">(){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>043</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">var</code> <code class="plain">scrolltop=jQuery(window).scrollTop()</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>044</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">if</code> <code class="plain">(!</code><code class="keyword">this</code><code class="plain">.cssfixedsupport)</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>045</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">this</code><code class="plain">.keepfixed()</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>046</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">this</code><code class="plain">.state.shouldvisible=(scrolltop&gt;=</code><code class="keyword">this</code><code class="plain">.setting.startline)? </code><code class="keyword">true</code> <code class="plain">: </code><code class="keyword">false</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>047</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">if</code> <code class="plain">(</code><code class="keyword">this</code><code class="plain">.state.shouldvisible &amp;&amp; !</code><code class="keyword">this</code><code class="plain">.state.isvisible){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>048</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">this</code><code class="plain">.$control.stop().animate({opacity:1}, </code><code class="keyword">this</code><code class="plain">.setting.fadeduration[0])</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>049</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">this</code><code class="plain">.state.isvisible=</code><code class="keyword">true</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>050</code></td>
<td class="content"><code class="spaces">  </code><code class="plain">}</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>051</code></td>
<td class="content"><code class="spaces">  </code><code class="keyword">else</code> <code class="keyword">if</code> <code class="plain">(</code><code class="keyword">this</code><code class="plain">.state.shouldvisible==</code><code class="keyword">false</code> <code class="plain">&amp;&amp; </code><code class="keyword">this</code><code class="plain">.state.isvisible){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>052</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">this</code><code class="plain">.$control.stop().animate({opacity:0}, </code><code class="keyword">this</code><code class="plain">.setting.fadeduration[1])</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>053</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">this</code><code class="plain">.state.isvisible=</code><code class="keyword">false</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>054</code></td>
<td class="content"><code class="spaces">  </code><code class="plain">}</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>055</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">},</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>056</code></td>
<td class="content"><code class="spaces"> </code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>057</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">init:</code><code class="keyword">function</code><code class="plain">(){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>058</code></td>
<td class="content"><code class="spaces">  </code><code class="plain">jQuery(document).ready(</code><code class="keyword">function</code><code class="plain">($){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>059</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">var</code> <code class="plain">mainobj=scrolltotop</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>060</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">var</code> <code class="plain">iebrws=document.all</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>061</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">mainobj.cssfixedsupport=!iebrws || iebrws &amp;&amp; document.compatMode==</code><code class="string">"CSS1Compat"</code> <code class="plain">&amp;&amp; window.XMLHttpRequest </code><code class="comments">//not IE or IE7+ browsers in standards mode</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>062</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">mainobj.$body=(window.opera)? (document.compatMode==</code><code class="string">"CSS1Compat"</code><code class="plain">? $(</code><code class="string">'html'</code><code class="plain">) : $(</code><code class="string">'body'</code><code class="plain">)) : $(</code><code class="string">'html,body'</code><code class="plain">)</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>063</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">mainobj.$control=$(</code><code class="string">'&lt;div id="topcontrol"&gt;'</code><code class="plain">+mainobj.controlHTML+</code><code class="string">'&lt;/div&gt;'</code><code class="plain">)</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>064</code></td>
<td class="content"><code class="spaces">    </code><code class="plain">.css({position:mainobj.cssfixedsupport? </code><code class="string">'fixed'</code> <code class="plain">: </code><code class="string">'absolute'</code><code class="plain">, bottom:mainobj.controlattrs.offsety, right:mainobj.controlattrs.offsetx, opacity:0, cursor:</code><code class="string">'pointer'</code><code class="plain">})</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>065</code></td>
<td class="content"><code class="spaces">    </code><code class="plain">.attr({title:</code><code class="string">'Scroll Back to Top'</code><code class="plain">})</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>066</code></td>
<td class="content"><code class="spaces">    </code><code class="plain">.click(</code><code class="keyword">function</code><code class="plain">(){mainobj.scrollup(); </code><code class="keyword">return</code> <code class="keyword">false</code><code class="plain">})</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>067</code></td>
<td class="content"><code class="spaces">    </code><code class="plain">.appendTo(</code><code class="string">'body'</code><code class="plain">)</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>068</code></td>
<td class="content"><code class="spaces">   </code><code class="keyword">if</code> <code class="plain">(document.all &amp;&amp; !window.XMLHttpRequest &amp;&amp; mainobj.$control.text()!=</code><code class="string">''</code><code class="plain">) </code><code class="comments">//loose check for IE6 and below, plus whether control contains any text</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>069</code></td>
<td class="content"><code class="spaces">    </code><code class="plain">mainobj.$control.css({width:mainobj.$control.width()}) </code><code class="comments">//IE6- seems to require an explicit width on a DIV containing text</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>070</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">mainobj.togglecontrol()</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>071</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">$(</code><code class="string">'a[href="'</code> <code class="plain">+ mainobj.anchorkeyword +</code><code class="string">'"]'</code><code class="plain">).click(</code><code class="keyword">function</code><code class="plain">(){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>072</code></td>
<td class="content"><code class="spaces">    </code><code class="plain">mainobj.scrollup()</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>073</code></td>
<td class="content"><code class="spaces">    </code><code class="keyword">return</code> <code class="keyword">false</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>074</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">})</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>075</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">$(window).bind(</code><code class="string">'scroll resize'</code><code class="plain">, </code><code class="keyword">function</code><code class="plain">(e){</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>076</code></td>
<td class="content"><code class="spaces">    </code><code class="plain">mainobj.togglecontrol()</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>077</code></td>
<td class="content"><code class="spaces">   </code><code class="plain">})</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>078</code></td>
<td class="content"><code class="spaces">  </code><code class="plain">})</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>079</code></td>
<td class="content"><code class="spaces"> </code><code class="plain">}</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>080</code></td>
<td class="content"><code class="plain">}</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>081</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>082</code></td>
<td class="content"><code class="plain">scrolltotop.init()</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>083</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>084</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>085</code></td>
<td class="content"><code class="plain">&lt;/script&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>086</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>087</code></td>
<td class="content"><code class="plain">&lt;/head&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>088</code></td>
<td class="content"><code class="plain">&lt;body&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>089</code></td>
<td class="content"><code class="plain">&lt;p&gt;&lt;h1&gt;基于jQuery的返回顶部按钮实现&lt;/h1&gt;&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>090</code></td>
<td class="content"><code class="plain">&lt;p&gt;工作需要一个会顶部，找了半天终于找到一个jQuery做的很相似，又好用的返回顶部功能。&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>091</code></td>
<td class="content"><code class="plain">&lt;p&gt;使用起来非常的方便，只需要把jQuery的核心文件jquery.min.js和实现功能的scrolltopcontrol.js文件引入就即可。样式可以自己调一下合适自己的。</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>092</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>093</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>094</code></td>
<td class="content"><code class="spaces"> </code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>095</code></td>
<td class="content"></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>096</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>097</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>098</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>099</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>100</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>101</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>102</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>103</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>104</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>105</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>106</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>107</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>108</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>109</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>110</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>111</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>112</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>113</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>114</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>115</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>116</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>117</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>118</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>119</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>120</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>121</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>122</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>123</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>124</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>125</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>126</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>127</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>128</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>129</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>130</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>131</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>132</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>133</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>134</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>135</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>136</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>137</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>138</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>139</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>140</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>141</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>142</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>143</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>144</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>145</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>146</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>147</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>148</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>149</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>150</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>151</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>152</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>153</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>154</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>155</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>156</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>157</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>158</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>159</code></td>
<td class="content"><code class="plain">&lt;p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>160</code></td>
<td class="content"><code class="plain">咱们测试的，为了增加高度，反映效果的文字</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>161</code></td>
<td class="content"><code class="plain">&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>162</code></td>
<td class="content"><code class="plain">&lt;p&gt;&lt;h1&gt;基于jQuery的返回顶部按钮实现&lt;/h1&gt;&lt;/p&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt1">
<table>
<tbody>
<tr>
<td class="number"><code>163</code></td>
<td class="content"><code class="plain">&lt;/body&gt;</code></td>
</tr>
</tbody>
</table>
</div>
<div class="line alt2">
<table>
<tbody>
<tr>
<td class="number"><code>164</code></td>
<td class="content"><code class="plain">&lt;/html&gt;</code></td>
</tr>
</tbody>
</table>
</div>
