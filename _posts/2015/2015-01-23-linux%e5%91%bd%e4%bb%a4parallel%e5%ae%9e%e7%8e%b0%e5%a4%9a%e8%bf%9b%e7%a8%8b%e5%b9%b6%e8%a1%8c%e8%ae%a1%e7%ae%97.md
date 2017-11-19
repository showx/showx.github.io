---
layout: post
title: linux命令parallel实现多进程并行计算
date: 2015-01-23 10:03
author: admin
comments: true
categories: []
---
<div>需求分析：</div>
<div></div>
<div>假设我们有三个进程A和B和C，分别对应三个运行脚本a.sh,b.sh,c.sh。</div>
<div>A和B两个进程是完全独立的。</div>
<div>C进行必须等待A进程和B进程都运行结束之后，才能启动C进程。</div>
<div></div>
<div>我们现在需要写一个脚本要运行这三个程序脚本</div>
<div></div>
<div>解决方法：</div>
<div></div>
<div>串联【不理想】：</div>
<div></div>
<div>
<div class="dp-highlighter bg_plain">
<div class="bar">
<div class="tools"><b>[plain]</b> <a class="ViewSource" title="view plain" href="http://blog.csdn.net/wuyanyi/article/details/14647641#">view plain</a><a class="CopyToClipboard" title="copy" href="http://blog.csdn.net/wuyanyi/article/details/14647641#">copy</a>
<div><embed id="ZeroClipboardMovie_1" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_1"></embed></div>
</div>
</div>
<ol start="1">
	<li class="alt">a.sh;</li>
	<li class="">b.sh;</li>
	<li class="alt">c.sh;</li>
</ol>
</div>
</div>
<div></div>
<div>但是如果这三个进程都要运行特别长的时间，如果10个小时左右。</div>
<div>这样的话运行总时间就需要30个小时。</div>
<div></div>
<div>因为我们知道A和B是相互独立的所以我们应该将A和B并联起来，然后再和C串联。</div>
<div>这样运行时间就优化成了20个小时。</div>
<div></div>
<div>并联的话我们不能使用后台运行的方式，如果将A或者B后台运行了，C需要等待A和B都运行结束了才启动，但是C不知道A和B什么时候结束。</div>
<div></div>
<div><strong>暂时能想到的最简单的办法是</strong>【<strong>引入parallel命令来解决</strong>】：</div>
<div></div>
<div>
<div class="dp-highlighter bg_plain">
<div class="bar">
<div class="tools"><b>[plain]</b> <a class="ViewSource" title="view plain" href="http://blog.csdn.net/wuyanyi/article/details/14647641#">view plain</a><a class="CopyToClipboard" title="copy" href="http://blog.csdn.net/wuyanyi/article/details/14647641#">copy</a>
<div><embed id="ZeroClipboardMovie_2" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_2"></embed></div>
</div>
</div>
<ol start="1">
	<li class="alt">parallel -j 2 "sh a.sh" "sh b.sh";</li>
	<li class="">sh c.sh</li>
</ol>
</div>
</div>
<div></div>
<div>这样A和B就通过parallel来进行并联，等到A和B都运行结束了，parallel这个命令也就运行结束。</div>
<div>然后再进行C程序。（满足我们的要求）</div>
&nbsp;
