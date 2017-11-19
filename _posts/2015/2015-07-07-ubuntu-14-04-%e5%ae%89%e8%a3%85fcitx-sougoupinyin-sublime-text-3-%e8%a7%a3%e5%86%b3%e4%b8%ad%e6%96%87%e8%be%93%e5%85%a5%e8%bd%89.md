---
layout: post
title: UBUNTU 14.04 安装fcitx-sougoupinyin, sublime text 3, 解决中文输入[轉]
date: 2015-07-07 10:59
author: admin
comments: true
categories: []
---
适用于新安装的系统，要是做了很多操作，那就不好说了
<ol>
	<li>换成163的源，更新，</li>
	<li>language support里添加中文，界面语言是不必设为中文的</li>
	<li>到http://pinyin.sogou.com/linux/?r=pinyin下载deb包安装</li>
	<li>language support里输入方式选为fcitx</li>
	<li>重启，sougoupinyin 安装成功</li>
	<li>安装sublime text 3
<pre class="prettyprint lang-bash prettyprinted" lang="bash"><code contenteditable="true"><span class="pln">sudo add</span><span class="pun">-</span><span class="pln">apt</span><span class="pun">-</span><span class="pln">repository ppa</span><span class="pun">:</span><span class="pln">webupd8team</span><span class="pun">/</span><span class="pln">sublime</span><span class="pun">-</span><span class="pln">text</span><span class="pun">-</span><span class="lit">3</span><span class="pln">
sudo apt</span><span class="pun">-</span><span class="pln">get update
sudo apt</span><span class="pun">-</span><span class="pln">get install sublime</span><span class="pun">-</span><span class="pln">text</span><span class="pun">-</span><span class="pln">installer</span></code></pre>
</li>
	<li>解决中文输入问题
<pre class="prettyprint lang-bash prettyprinted" lang="bash"><code contenteditable="true"><span class="pln">cd </span><span class="pun">/</span><span class="pln">opt</span><span class="pun">/</span><span class="pln">sublime_text
sudo touch sublime_imfix</span><span class="pun">.</span><span class="pln">c
sudo gedit sublime_imfix</span><span class="pun">.</span><span class="pln">c</span></code></pre>
然后把以下内容粘贴进去并且保存
<pre class="prettyprint lang-c prettyprinted" lang="c"><code contenteditable="true"><span class="com">/*
sublime-imfix.c
Use LD_PRELOAD to interpose some function to fix sublime input method support for linux.
By Cjacker Huang &lt;jianzhong.huang at i-soft.com.cn&gt;

gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
LD_PRELOAD=./libsublime-imfix.so sublime_text
*/</span>
<span class="com">#include</span> <span class="str">&lt;gtk/gtk.h&gt;</span>
<span class="com">#include</span> <span class="str">&lt;gdk/gdkx.h&gt;</span>
<span class="kwd">typedef</span> <span class="typ">GdkSegment</span> <span class="typ">GdkRegionBox</span><span class="pun">;</span>

<span class="kwd">struct</span> <span class="typ">_GdkRegion</span>
<span class="pun">{</span>
  <span class="kwd">long</span><span class="pln"> size</span><span class="pun">;</span>
  <span class="kwd">long</span><span class="pln"> numRects</span><span class="pun">;</span>
  <span class="typ">GdkRegionBox</span> <span class="pun">*</span><span class="pln">rects</span><span class="pun">;</span>
  <span class="typ">GdkRegionBox</span><span class="pln"> extents</span><span class="pun">;</span>
<span class="pun">};</span>

<span class="typ">GtkIMContext</span> <span class="pun">*</span><span class="pln">local_context</span><span class="pun">;</span>

<span class="kwd">void</span><span class="pln">
gdk_region_get_clipbox </span><span class="pun">(</span><span class="kwd">const</span> <span class="typ">GdkRegion</span> <span class="pun">*</span><span class="pln">region</span><span class="pun">,</span>
            <span class="typ">GdkRectangle</span>    <span class="pun">*</span><span class="pln">rectangle</span><span class="pun">)</span>
<span class="pun">{</span><span class="pln">
  g_return_if_fail </span><span class="pun">(</span><span class="pln">region </span><span class="pun">!=</span><span class="pln"> NULL</span><span class="pun">);</span><span class="pln">
  g_return_if_fail </span><span class="pun">(</span><span class="pln">rectangle </span><span class="pun">!=</span><span class="pln"> NULL</span><span class="pun">);</span><span class="pln">

  rectangle</span><span class="pun">-&gt;</span><span class="pln">x </span><span class="pun">=</span><span class="pln"> region</span><span class="pun">-&gt;</span><span class="pln">extents</span><span class="pun">.</span><span class="pln">x1</span><span class="pun">;</span><span class="pln">
  rectangle</span><span class="pun">-&gt;</span><span class="pln">y </span><span class="pun">=</span><span class="pln"> region</span><span class="pun">-&gt;</span><span class="pln">extents</span><span class="pun">.</span><span class="pln">y1</span><span class="pun">;</span><span class="pln">
  rectangle</span><span class="pun">-&gt;</span><span class="pln">width </span><span class="pun">=</span><span class="pln"> region</span><span class="pun">-&gt;</span><span class="pln">extents</span><span class="pun">.</span><span class="pln">x2 </span><span class="pun">-</span><span class="pln"> region</span><span class="pun">-&gt;</span><span class="pln">extents</span><span class="pun">.</span><span class="pln">x1</span><span class="pun">;</span><span class="pln">
  rectangle</span><span class="pun">-&gt;</span><span class="pln">height </span><span class="pun">=</span><span class="pln"> region</span><span class="pun">-&gt;</span><span class="pln">extents</span><span class="pun">.</span><span class="pln">y2 </span><span class="pun">-</span><span class="pln"> region</span><span class="pun">-&gt;</span><span class="pln">extents</span><span class="pun">.</span><span class="pln">y1</span><span class="pun">;</span>
  <span class="typ">GdkRectangle</span><span class="pln"> rect</span><span class="pun">;</span><span class="pln">
  rect</span><span class="pun">.</span><span class="pln">x </span><span class="pun">=</span><span class="pln"> rectangle</span><span class="pun">-&gt;</span><span class="pln">x</span><span class="pun">;</span><span class="pln">
  rect</span><span class="pun">.</span><span class="pln">y </span><span class="pun">=</span><span class="pln"> rectangle</span><span class="pun">-&gt;</span><span class="pln">y</span><span class="pun">;</span><span class="pln">
  rect</span><span class="pun">.</span><span class="pln">width </span><span class="pun">=</span> <span class="lit">0</span><span class="pun">;</span><span class="pln">
  rect</span><span class="pun">.</span><span class="pln">height </span><span class="pun">=</span><span class="pln"> rectangle</span><span class="pun">-&gt;</span><span class="pln">height</span><span class="pun">;</span> 
  <span class="com">//The caret width is 2; </span>
  <span class="com">//Maybe sometimes we will make a mistake, but for most of the time, it should be the caret.</span>
  <span class="kwd">if</span><span class="pun">(</span><span class="pln">rectangle</span><span class="pun">-&gt;</span><span class="pln">width </span><span class="pun">==</span> <span class="lit">2</span> <span class="pun">&amp;&amp;</span><span class="pln"> GTK_IS_IM_CONTEXT</span><span class="pun">(</span><span class="pln">local_context</span><span class="pun">))</span> <span class="pun">{</span><span class="pln">
        gtk_im_context_set_cursor_location</span><span class="pun">(</span><span class="pln">local_context</span><span class="pun">,</span><span class="pln"> rectangle</span><span class="pun">);</span>
  <span class="pun">}</span>
<span class="pun">}</span>

<span class="com">//this is needed, for example, if you input something in file dialog and return back the edit area</span>
<span class="com">//context will lost, so here we set it again.</span>

<span class="kwd">static</span> <span class="typ">GdkFilterReturn</span><span class="pln"> event_filter </span><span class="pun">(</span><span class="typ">GdkXEvent</span> <span class="pun">*</span><span class="pln">xevent</span><span class="pun">,</span> <span class="typ">GdkEvent</span> <span class="pun">*</span><span class="pln">event</span><span class="pun">,</span><span class="pln"> gpointer im_context</span><span class="pun">)</span>
<span class="pun">{</span>
    <span class="typ">XEvent</span> <span class="pun">*</span><span class="pln">xev </span><span class="pun">=</span> <span class="pun">(</span><span class="typ">XEvent</span> <span class="pun">*)</span><span class="pln">xevent</span><span class="pun">;</span>
    <span class="kwd">if</span><span class="pun">(</span><span class="pln">xev</span><span class="pun">-&gt;</span><span class="pln">type </span><span class="pun">==</span> <span class="typ">KeyRelease</span> <span class="pun">&amp;&amp;</span><span class="pln"> GTK_IS_IM_CONTEXT</span><span class="pun">(</span><span class="pln">im_context</span><span class="pun">))</span> <span class="pun">{</span>
       <span class="typ">GdkWindow</span> <span class="pun">*</span><span class="pln"> win </span><span class="pun">=</span><span class="pln"> g_object_get_data</span><span class="pun">(</span><span class="pln">G_OBJECT</span><span class="pun">(</span><span class="pln">im_context</span><span class="pun">),</span><span class="str">"window"</span><span class="pun">);</span>
       <span class="kwd">if</span><span class="pun">(</span><span class="pln">GDK_IS_WINDOW</span><span class="pun">(</span><span class="pln">win</span><span class="pun">))</span><span class="pln">
         gtk_im_context_set_client_window</span><span class="pun">(</span><span class="pln">im_context</span><span class="pun">,</span><span class="pln"> win</span><span class="pun">);</span>
    <span class="pun">}</span>
    <span class="kwd">return</span><span class="pln"> GDK_FILTER_CONTINUE</span><span class="pun">;</span>
<span class="pun">}</span>

<span class="kwd">void</span><span class="pln"> gtk_im_context_set_client_window </span><span class="pun">(</span><span class="typ">GtkIMContext</span> <span class="pun">*</span><span class="pln">context</span><span class="pun">,</span>
          <span class="typ">GdkWindow</span>    <span class="pun">*</span><span class="pln">window</span><span class="pun">)</span>
<span class="pun">{</span>
  <span class="typ">GtkIMContextClass</span> <span class="pun">*</span><span class="pln">klass</span><span class="pun">;</span><span class="pln">
  g_return_if_fail </span><span class="pun">(</span><span class="pln">GTK_IS_IM_CONTEXT </span><span class="pun">(</span><span class="pln">context</span><span class="pun">));</span><span class="pln">
  klass </span><span class="pun">=</span><span class="pln"> GTK_IM_CONTEXT_GET_CLASS </span><span class="pun">(</span><span class="pln">context</span><span class="pun">);</span>
  <span class="kwd">if</span> <span class="pun">(</span><span class="pln">klass</span><span class="pun">-&gt;</span><span class="pln">set_client_window</span><span class="pun">)</span><span class="pln">
    klass</span><span class="pun">-&gt;</span><span class="pln">set_client_window </span><span class="pun">(</span><span class="pln">context</span><span class="pun">,</span><span class="pln"> window</span><span class="pun">);</span>

  <span class="kwd">if</span><span class="pun">(!</span><span class="pln">GDK_IS_WINDOW </span><span class="pun">(</span><span class="pln">window</span><span class="pun">))</span>
    <span class="kwd">return</span><span class="pun">;</span><span class="pln">
  g_object_set_data</span><span class="pun">(</span><span class="pln">G_OBJECT</span><span class="pun">(</span><span class="pln">context</span><span class="pun">),</span><span class="str">"window"</span><span class="pun">,</span><span class="pln">window</span><span class="pun">);</span>
  <span class="typ">int</span><span class="pln"> width </span><span class="pun">=</span><span class="pln"> gdk_window_get_width</span><span class="pun">(</span><span class="pln">window</span><span class="pun">);</span>
  <span class="typ">int</span><span class="pln"> height </span><span class="pun">=</span><span class="pln"> gdk_window_get_height</span><span class="pun">(</span><span class="pln">window</span><span class="pun">);</span>
  <span class="kwd">if</span><span class="pun">(</span><span class="pln">width </span><span class="pun">!=</span> <span class="lit">0</span> <span class="pun">&amp;&amp;</span><span class="pln"> height </span><span class="pun">!=</span><span class="lit">0</span><span class="pun">)</span> <span class="pun">{</span><span class="pln">
    gtk_im_context_focus_in</span><span class="pun">(</span><span class="pln">context</span><span class="pun">);</span><span class="pln">
    local_context </span><span class="pun">=</span><span class="pln"> context</span><span class="pun">;</span>
  <span class="pun">}</span><span class="pln">
  gdk_window_add_filter </span><span class="pun">(</span><span class="pln">window</span><span class="pun">,</span><span class="pln"> event_filter</span><span class="pun">,</span><span class="pln"> context</span><span class="pun">);</span> 
<span class="pun">}</span></code></pre>
再执行：
<pre class="prettyprint lang-bash prettyprinted" lang="bash"><code contenteditable="true"><span class="pln">sudo apt</span><span class="pun">-</span><span class="pln">get install build</span><span class="pun">-</span><span class="pln">essential libgtk2</span><span class="pun">.</span><span class="lit">0</span><span class="pun">-</span><span class="pln">dev
sudo gcc </span><span class="pun">-</span><span class="pln">shared </span><span class="pun">-</span><span class="pln">o libsublime</span><span class="pun">-</span><span class="pln">imfix</span><span class="pun">.</span><span class="pln">so sublime_imfix</span><span class="pun">.</span><span class="pln">c  </span><span class="str">`pkg-config --libs --cflags gtk+-2.0`</span> <span class="pun">-</span><span class="pln">fPIC</span></code></pre>
再编辑sublime快捷方式
<pre class="prettyprint lang-bash prettyprinted" lang="bash"><code contenteditable="true"><span class="pln">sudo gedit </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="pln">share</span><span class="pun">/</span><span class="pln">applications</span><span class="pun">/</span><span class="pln">sublime</span><span class="pun">-</span><span class="pln">text</span><span class="pun">.</span><span class="pln">desktop</span></code></pre>
把以下内容替换原来内容并且保存
<pre class="prettyprint lang-bash prettyprinted" lang="bash"><code contenteditable="true"><span class="pun">[</span><span class="typ">Desktop</span> <span class="typ">Entry</span><span class="pun">]</span>
<span class="typ">Version</span><span class="pun">=</span><span class="lit">1.0</span>
<span class="typ">Type</span><span class="pun">=</span><span class="typ">Application</span>
<span class="typ">Name</span><span class="pun">=</span><span class="typ">Sublime</span> <span class="typ">Text</span>
<span class="typ">GenericName</span><span class="pun">=</span><span class="typ">Text</span> <span class="typ">Editor</span>
<span class="typ">Comment</span><span class="pun">=</span><span class="typ">Sophisticated</span><span class="pln"> text editor </span><span class="kwd">for</span><span class="pln"> code</span><span class="pun">,</span><span class="pln"> markup and prose
</span><span class="typ">Exec</span><span class="pun">=</span><span class="pln">bash </span><span class="pun">-</span><span class="pln">c </span><span class="str">'LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so /opt/sublime_text/sublime_text'</span> <span class="pun">%</span><span class="pln">F
</span><span class="typ">Terminal</span><span class="pun">=</span><span class="pln">false
</span><span class="typ">MimeType</span><span class="pun">=</span><span class="pln">text</span><span class="pun">/</span><span class="pln">plain</span><span class="pun">;</span>
<span class="typ">Icon</span><span class="pun">=</span><span class="pln">sublime</span><span class="pun">-</span><span class="pln">text
</span><span class="typ">Categories</span><span class="pun">=</span><span class="typ">TextEditor</span><span class="pun">;</span><span class="typ">Development</span><span class="pun">;</span><span class="typ">Utility</span><span class="pun">;</span>
<span class="typ">StartupNotify</span><span class="pun">=</span><span class="pln">true
</span><span class="typ">Actions</span><span class="pun">=</span><span class="typ">Window</span><span class="pun">;</span><span class="typ">Document</span><span class="pun">;</span><span class="pln">

X</span><span class="pun">-</span><span class="typ">Desktop</span><span class="pun">-</span><span class="typ">File</span><span class="pun">-</span><span class="typ">Install</span><span class="pun">-</span><span class="typ">Version</span><span class="pun">=</span><span class="lit">0.22</span>

<span class="pun">[</span><span class="typ">Desktop</span> <span class="typ">Action</span> <span class="typ">Window</span><span class="pun">]</span>
<span class="typ">Name</span><span class="pun">=</span><span class="typ">New</span> <span class="typ">Window</span>
<span class="typ">Exec</span><span class="pun">=</span><span class="pln">bash </span><span class="pun">-</span><span class="pln">c </span><span class="str">'LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so /opt/sublime_text/sublime_text'</span> <span class="pun">-</span><span class="pln">n
</span><span class="typ">OnlyShowIn</span><span class="pun">=</span><span class="typ">Unity</span><span class="pun">;</span>

<span class="pun">[</span><span class="typ">Desktop</span> <span class="typ">Action</span> <span class="typ">Document</span><span class="pun">]</span>
<span class="typ">Name</span><span class="pun">=</span><span class="typ">New</span> <span class="typ">File</span>
<span class="typ">Exec</span><span class="pun">=</span><span class="pln">bash </span><span class="pun">-</span><span class="pln">c </span><span class="str">'LD_PRELOAD=/opt/sublime_text/libsublime-imfix.so /opt/sublime_text/sublime_text'</span> <span class="pun">--</span><span class="pln">command new_file
</span><span class="typ">OnlyShowIn</span><span class="pun">=</span><span class="typ">Unity</span><span class="pun">;</span></code></pre>
完工。</li>
	<li>可选，使用我的sublime配置文件
<p class="bold color-red">务必至少打开一次sublime text 3, 并且在关闭状态下执行以下操作</p>

<pre class="prettyprint lang-bash prettyprinted" lang="bash"><code contenteditable="true"><span class="pln">cd </span><span class="pun">~/.</span><span class="pln">config</span><span class="pun">/</span><span class="pln">sublime</span><span class="pun">-</span><span class="pln">text</span><span class="pun">-</span><span class="lit">3</span><span class="pln">
rm </span><span class="pun">-</span><span class="pln">rf </span><span class="typ">Installed</span><span class="pln">\ </span><span class="typ">Packages</span><span class="pln">
rm </span><span class="typ">Packages</span><span class="pun">/</span><span class="typ">User</span><span class="pun">/</span><span class="typ">Package</span><span class="pln">\ </span><span class="typ">Control</span><span class="pun">.</span><span class="pln">sublime</span><span class="pun">-</span><span class="pln">settings
rm </span><span class="typ">Packages</span><span class="pun">/</span><span class="typ">User</span><span class="pun">/</span><span class="typ">Preferences</span><span class="pun">.</span><span class="pln">sublime</span><span class="pun">-</span><span class="pln">settings
git clone https</span><span class="pun">://</span><span class="pln">github</span><span class="pun">.</span><span class="pln">com</span><span class="pun">/</span><span class="pln">zxdong262</span><span class="pun">/</span><span class="pln">sublime3</span><span class="pun">-</span><span class="pln">setting</span><span class="pun">.</span><span class="pln">git
cp </span><span class="pun">-</span><span class="pln">R sublime3</span><span class="pun">-</span><span class="pln">setting</span><span class="pun">/*</span> <span class="pun">.</span></code></pre>
</li>
</ol>
