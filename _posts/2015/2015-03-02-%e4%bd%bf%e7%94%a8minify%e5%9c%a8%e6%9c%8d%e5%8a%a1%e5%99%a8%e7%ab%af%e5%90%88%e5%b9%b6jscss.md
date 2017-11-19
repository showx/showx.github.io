---
layout: post
title: 使用Minify在服务器端合并js/css
date: 2015-03-02 18:15
author: admin
comments: true
categories: []
---
Minify 用于服务器端的JavaScript 和 CSS的合并压缩。

1. 首先从 Google code下载 Minify

<a href="http://code.google.com/p/minify/wiki/UserGuide" target="_blank">http://code.google.com/p/minify/wiki/UserGuide </a>

2. 上传至网站根目录下，当然，也可以你所指定的位置，但是需要注意的是需要修改Minify。

3. 修改Nginx配置，可以参见我前面的文章：

<a href="http://blog.csdn.net/spring21st/article/details/7220356">Minify在Nginx上的rewrite配置 </a>
P.S. 添加重定向目的在于合并压缩后的js和css代码路径不携带?

&nbsp;

4.  修改Minify配置文件

&nbsp;
<div>
<div class="dp-highlighter bg_php">
<div class="bar">
<div class="tools"><b>[php]</b> <a class="ViewSource" title="view plain" href="http://blog.csdn.net/spring21st/article/details/7222851#">view plain</a><a class="CopyToClipboard" title="copy" href="http://blog.csdn.net/spring21st/article/details/7222851#">copy</a>
<div><embed id="ZeroClipboardMovie_1" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_1"></embed></div>
</div>
</div>
<ol class="dp-c" start="1">
	<li class="alt">&lt;?php</li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Configuration for default Minify application</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * @package Minify</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * In 'debug' mode, Minify can combine fileswith no minification and</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * add comments to indicate line #s of theoriginal files.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * To allow debugging, set this option to trueand add "&amp;debug=1" to</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * a URI. E.g./min/?f=script1.js,script2.js&amp;debug=1</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_allowDebugFlag</span>= false;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Set to true to log messages to FirePHP(Firefox Firebug addon).</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Set to false for no error logging (Minifymay be slightly faster).</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * @link http://www.firephp.org/</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * If you want to use a custom error logger,set this to your logger</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * instance. Your object should have a methodlog(string $message).</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * @todo cache system does not have errorlogging yet.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_errorLogger</span> =false;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Allow use of the Minify URI Builder app. Ifyou no longer need</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * this, set to false.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> **/</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_enableBuilder</span> =true;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * For best performance, specify your tempdirectory here. Otherwise Minify</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * will have to load extra code to guess. Someexamples below:</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment">//$min_cachePath ='c:\\WINDOWS\\Temp';</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_cachePath</span>= <span class="string">'/tmp'</span>;</li>
	<li class=""></li>
	<li class="alt"><span class="comment">//$min_cachePath =preg_replace('/^\\d+;/', '', session_save_path());</span></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Leave an empty string to use PHP's$_SERVER['DOCUMENT_ROOT'].</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * On some servers, this value may bemisconfigured or missing. If so, set this</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * to your full document root path with notrailing slash.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * E.g. '/home/accountname/public_html' or'c:\\xampp\\htdocs'</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * If /min/ is directly inside your documentroot, just uncomment the</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * second line. The third line might work onsome Apache servers.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_documentRoot</span>= <span class="string">''</span>;</li>
	<li class=""></li>
	<li class="alt"><span class="comment">//$min_documentRoot =substr(__FILE__, 0, strlen(__FILE__) - 15);</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment">//$min_documentRoot =$_SERVER['SUBDOMAIN_DOCUMENT_ROOT'];</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment">//$min_documentRoot ='/var/www/cychai/Cache/';</span></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Cache file locking. Set to false iffilesystem is NFS. On at least one</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * NFS system flock-ing attempts stalled PHPfor 30 seconds!</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_cacheFileLocking</span>= true;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Combining multiple CSS files can place@import declarations after rules, which</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * is invalid. Minify will attempt to detectwhen this happens and place a</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * warning comment at the top of the CSSoutput. To resolve this you can either</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * move the @imports within your CSS files, orenable this option, which will</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * move all @imports to the top of the output.Note that moving @imports could</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * affect CSS values (which is why this optionis disabled by default).</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_serveOptions</span>[<span class="string">'bubbleCssImports'</span>]= false;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Maximum age of browser cache in seconds.After this period, the browser</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * will send another conditional GET. Use alonger period for lower traffic</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * but you may want to shorten this beforemaking changes if it's crucial</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * those changes are seen immediately.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Note: Despite this setting, if you include anumber at the end of the</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * querystring, maxAge will be set to one year.E.g. /min/f=hello.css&amp;123456</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_serveOptions</span>[<span class="string">'maxAge'</span>]= 315360000;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * If you'd like to restrict the "f"option to files within/below</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * particular directories below DOCUMENT_ROOT,set this here.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * You will still need to include the directoryin the</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * f or b GET parameters.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * // = shortcut for DOCUMENT_ROOT</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment">//$min_serveOptions['minApp']['allowDirs']= array('//js', '//css');</span></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Set to true to disable the "f" GETparameter for specifying files.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Only the "g" parameter will beconsidered.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_serveOptions</span>[<span class="string">'minApp'</span>][<span class="string">'groupsOnly'</span>]= false;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Maximum # of files that can be specified inthe "f" GET parameter</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_serveOptions</span>[<span class="string">'minApp'</span>][<span class="string">'maxFiles'</span>]= 50;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * If you minify CSS files stored in symlink-eddirectories, the URI rewriting</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * algorithm can fail. To prevent this, providean array of link paths to</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * target paths, where the link paths arewithin the document root.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Because paths need to be normalized for thisto work, use "//" to substitute</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * the doc root in the link paths (the arraykeys). E.g.:</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * &lt;code&gt;</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * array('//symlink' =&gt; '/real/target/path')// unix</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * array('//static' =&gt;'D:\\staticStorage')  // Windows</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * &lt;/code&gt;</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_symlinks</span> =<span class="keyword">array</span>();</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * If you upload files from Windows to anon-Windows server, Windows may report</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * incorrect mtimes for the files. This maycause Minify to keep serving stale</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * cache files when source file changes aremade too frequently (e.g. more than</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * once an hour).</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Immediately after modifying and uploading afile, use the touch command to</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * update the mtime on the server. If the mtimejumps ahead by a number of hours,</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * set this variable to that number. If themtime moves back, this should not be</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * needed.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> *</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * In the Windows SFTP client WinSCP, there'san option that may fix this</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * issue without changing the variable below.Under login &gt; environment,</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * select the option "Adjust remotetimestamp with DST".</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * @linkhttp://winscp.net/eng/docs/ui_login_environment#daylight_saving_time</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_uploaderHoursBehind</span>= 0;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">/**</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * Path to Minify's lib folder. If you happento move it, change</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> * this accordingly.</span></li>
	<li class=""></li>
	<li class="alt"><span class="comment"> */</span></li>
	<li class=""></li>
	<li class="alt"><span class="vars">$min_libPath</span> =dirname(<span class="keyword">__FILE__</span>) . <span class="string">'/lib'</span>;</li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"></li>
	<li class=""></li>
	<li class="alt"><span class="comment">// try to disableoutput_compression (may not have an effect)</span></li>
	<li class=""></li>
	<li class="alt"><span class="func">ini_set</span>(<span class="string">'zlib.output_compression'</span>,<span class="string">'0'</span>);</li>
</ol>
</div>
</div>
5. 重启Nginx服务。

6. 使用Minify其它页面的JavaScript文件，src地址格式为 <em>/min/f=js/[javascript文件名]</em>

<em> </em>

如：
<div>

&lt;scripttype="text/javascript" src="<strong>/min/f=</strong>js/common.js"&gt;&lt;/script&gt;

</div>
&nbsp;

对于合并多个JavaScript文件，可以通过Minify服务进行合并：

开发环境访问地址：

<a href="http://cychai.wap.dangdang.com/min/builder/">http://localhost/min/builder/</a>

填写需要合并的JavaScript文件，Update后，会生成请求的URI地址，在相应页面引用即可。
