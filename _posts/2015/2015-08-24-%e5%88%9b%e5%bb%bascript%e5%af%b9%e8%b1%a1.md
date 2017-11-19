---
layout: post
title: 创建script对象
date: 2015-08-24 08:42
author: admin
comments: true
categories: []
---
创建script对象
oScript = document.createElement("script");
oScript.setAttribute("src", url); // 内容来自技术世界www.js4j.com 专业技术//
oScript.setAttribute("id",id);
oScript.setAttribute("type","text/javascript");
oScript.setAttribute("language","javascript");
head.appendChild(oScript);
return oScript;

&nbsp;

&nbsp;

要实现动态加载JS脚本有4种方法：

<b>1、直接document.write</b>
<b>这里重新温习Document.write()的 用法，必须在加载页面的过程中使用，否则将复写整个页面，这里我碰到的问题是，如果用一些异步方法把内容写进页面Body中是不行的，应为IE无法被我们 的脚本所阻塞，无法要求加载过程中断，等待我们取得必要的信息，这时候我们将无法保证加载后脚本具体执行的情况。</b>
<b>&lt;script language=</b><b>"javascript"</b><b>&gt;</b>
<b></b>
<b>     document.write(</b><b>"&lt;script src="test.js"&gt;&lt;/script&gt;"</b><b>);</b>
<b></b>
<b>&lt;/script&gt;</b>
<b>2、动态改变已有script的src属性</b>
<b>&lt;script src=</b><b>""</b><b> id=</b><b>"s1"</b><b>&gt;&lt;/script&gt;</b>
<b></b>
<b>&lt;script language=</b><b>"javascript"</b><b>&gt;</b>
<b></b>
<b>     s1.src=</b><b>"test.js"</b>
<b></b>
<b>&lt;/script&gt;</b>
<b>3、动态创建script元素</b>
<b>&lt;script&gt;</b>
<b></b>
<b>    </b><b>var</b><b> oHead = document.getElementsByTagName(</b><b>"HEAD"</b><b>).item(0);</b>
<b></b>
<b>    </b><b>var</b><b> oScript= document.createElement(</b><b>"script"</b><b>);</b>
<b></b>
<b>     oScript.type = </b><b>"text/javascript"</b><b>;</b>
<b></b>
<b>     oScript.src=</b><b>"test.js"</b><b>;</b>
<b></b>
<b>     oHead.appendChild( oScript);</b>
<b></b>
<b>&lt;/script&gt;</b>
<b>　　这三种方法都是异步执行的，也就是说，在加载这些脚本的同时，主页面的脚本继续运行，如果用以上的方法，那下面的代码将得不到预期的效果。</b>
<b>要动态加载的JS脚本：a.js，以下是该文件的内容。</b>
<b>var</b> <b>str = </b><b>"中国"</b><b>;</b>
<b></b>
<b>alert( </b><b>"这是a.js中的变量："</b><b> + str );</b>
<b></b>
<b></b>
<b>主页面代码：</b>
<b></b>
<b></b>
<b>&lt;script language=</b><b>"JavaScript"</b><b>&gt;</b>
<b></b>
<b>function</b><b> LoadJS( id, fileUrl )</b>
<b></b>
<b>{</b>
<b></b>
<b>   </b><b>var</b><b> scriptTag = document.getElementById( id );</b>
<b></b>
<b>    </b><b>var</b><b> oHead = document.getElementsByTagName(</b><b>"HEAD"</b><b>).item(0);</b>
<b></b>
<b>   </b><b>var</b><b> oScript= document.createElement(</b><b>"script"</b><b>);</b>
<b></b>
<b></b>
<b></b>
<b>   </b><b>if</b><b> ( scriptTag   ) oHead.removeChild( scriptTag   );</b>
<b></b>
<b>     oScript.id = id;</b>
<b></b>
<b>    oScript.type = </b><b>"text/javascript"</b><b>;</b>
<b></b>
<b>     oScript.src=fileUrl ;</b>
<b></b>
<b>    oHead.appendChild( oScript);</b>
<b></b>
<b>}</b>
<b></b>
<b></b>
<b></b>
<b>LoadJS( </b><b>"a.js"</b><b> );</b>
<b></b>
<b></b>
<b></b>
<b>alert( </b><b>"主页面动态加载a.js并取其中的变量："</b><b> + str );</b>
<b></b>
<b>&lt;/script&gt;</b>
<b>上述代码执行后 a.js 的 alert 执行并弹出消息，</b>
<b></b>
<b></b>
<b></b>
<b>但是 主页面产生了错误，没有弹出对话框。</b><u><b>原因是 "str" 未定义</b></u><b>，为什么呢？因为主页面在取 str 的时候 a.js 并没有完全加载成功。遇到需要同步执行脚本的时候，可以用下面的第四种方法。</b>
<b>4、原理：用XMLHTTP取得要脚本的内容，再创建 Script 对象。</b>
<b></b>
<u><b>注意：a.js必须用UTF8编码保存，要不会出错。因为服务器与XML使用UTF8编码传送数据。这里还有一种方法，在WEB Server上设置默认编码，如果JS保存为GB2312 就在服务器设置默认的文件编码为GB2312，这样就不会出错了。</b></u>
<b></b>
<b>主页面代码：</b>
<b>&lt;script language=</b><b>"JavaScript"</b><b>&gt;</b>
<b></b>
<b>function</b><b> GetHttpRequest()</b>
<b></b>
<b>{</b>
<b></b>
<b>    if</b><b> ( window.XMLHttpRequest ) </b><b>// Gecko</b>
<b></b>
<b>        </b><b>return</b><b> </b><b>new</b><b> XMLHttpRequest() ;</b>
<b></b>
<b>    </b><b>else</b><b> </b><b>if</b><b> ( window.ActiveXObject ) </b><b>// IE</b>
<b></b>
<b>        </b><b>return</b><b> </b><b>new</b><b> ActiveXObject(</b><b>"MsXml2.XmlHttp"</b><b>) ;</b>
<b></b>
<b>}</b>
<b></b>
<b></b>
<b></b>
<b>function</b><b> AjaxPage(sId, url){</b>
<b></b>
<b>    var</b><b> oXmlHttp = GetHttpRequest() ;</b>
<b></b>
<b></b>
<b></b>
<b>    oXmlHttp.OnReadyStateChange = </b><b>function</b><b>()  </b>
<b></b>
<b>     {</b>
<b></b>
<b>        </b><b>if</b><b> ( oXmlHttp.readyState == 4 )</b>
<b></b>
<b>        {</b>
<b></b>
<b>            </b><b>if</b><b> ( oXmlHttp.status == 200 || oXmlHttp.status == 304 )</b>
<b></b>
<b>            {</b>
<b></b>
<b>                IncludeJS( sId, url, oXmlHttp.responseText );</b>
<b></b>
<b>            }</b>
<b></b>
<b>            </b><b>else</b>
<b></b>
<b>             {</b>
<b></b>
<b>                 alert( </b><b>"XML request error: "</b><b> + oXmlHttp.statusText + </b><b>" ("</b><b> + oXmlHttp.status + </b><b>")"</b><b> ) ;</b>
<b></b>
<b>             }</b>
<b></b>
<b>        }</b>
<b></b>
<b>    }</b>
<b></b>
<b></b>
<b></b>
<b>    oXmlHttp.open(</b><b>"GET"</b><b>, url, </b><b>true</b><b>);</b>
<b></b>
<b>    oXmlHttp.send(</b><b>null</b><b>);</b>
<b></b>
<b>}</b>
<b></b>
<b></b>
<b></b>
<b>function</b><b> IncludeJS(sId, fileUrl, source)</b>
<b></b>
<b>{</b>
<b></b>
<b>    if</b><b> ( ( source != </b><b>null</b><b> ) &amp;&amp; ( !document.getElementById( sId ) ) ){</b>
<b></b>
<b>        var</b><b> oHead = document.getElementsByTagName(</b><b>"HEAD"</b><b>).item(0);</b>
<b></b>
<b>        var</b><b> oScript = document.createElement( </b><b>"script"</b><b> );</b>
<b></b>
<b></b>
<b></b>
<b>        oScript.language = </b><b>"javascript"</b><b>;</b>
<b></b>
<b>        oScript.type = </b><b>"text/javascript"</b><b>;</b>
<b></b>
<b>        oScript.id = sId;</b>
<b></b>
<b>        oScript.defer = </b><b>true</b><b>;</b>
<b></b>
<b>        oScript.text = source;</b>
<b></b>
<b></b>
<b></b>
<b>        oHead.appendChild( oScript );</b>
<b></b>
<b>    }</b>
<b></b>
<b>}</b>
<b></b>
<b></b>
<b></b>
<b>AjaxPage( </b><b>"scrA"</b><b>, </b><b>"b.js"</b><b> );</b>
<b></b>
<b></b>
<b></b>
<b>alert( </b><b>"主页面动态加载JS脚本。"</b><b>);</b>
<b></b>
<b>alert( </b><b>"主页面动态加载a.js并取其中的变量："</b><b> + str );</b>
<b></b>
<b>&lt;/script&gt;</b>
