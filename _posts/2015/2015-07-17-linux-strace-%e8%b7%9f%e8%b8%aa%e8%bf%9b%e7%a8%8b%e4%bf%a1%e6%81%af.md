---
layout: post
title: Linux strace 跟踪进程信息
date: 2015-07-17 09:48
author: admin
comments: true
categories: []
---
简介
strace常用来跟踪进程执行时的系统调用和所接收的信号。 在Linux世界，进程不能直接访问硬件设备，当进程需要访问硬件设备(比如读取磁盘文件，接收网络数据等等)时，必须由用户态模式切换至内核态模式，通 过系统调用访问硬件设备。strace可以跟踪到一个进程产生的系统调用,包括参数，返回值，执行消耗的时间。

输出参数含义

root@Ubuntu:/usr# strace cat /dev/null
execve("/bin/cat", ["cat", "/dev/null"], [/* 22 vars */]) = 0
brk(0) = 0xab1000
access("/etc/ld.so.nohwcap", F_OK) = -1 ENOENT (No such file or directory)
mmap(NULL, 8192, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0x7f29379a7000
access("/etc/ld.so.preload", R_OK) = -1 ENOENT (No such file or directory)
...
brk(0) = 0xab1000
brk(0xad2000) = 0xad2000
fstat(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 0), ...}) = 0
open("/dev/null", O_RDONLY) = 3
fstat(3, {st_mode=S_IFCHR|0666, st_rdev=makedev(1, 3), ...}) = 0
read(3, "", 32768) = 0
close(3) = 0
close(1) = 0
close(2) = 0
exit_group(0) = ?
每一行都是一条系统调用，等号左边是系统调用的函数名及其参数，右边是该调用的返回值。
strace 显示这些调用的参数并返回符号形式的值。strace 从内核接收信息，而且不需要以任何特殊的方式来构建内核。

strace参数

-c 统计每一系统调用的所执行的时间,次数和出错的次数等.
-d 输出strace关于标准错误的调试信息.
-f 跟踪由fork调用所产生的子进程.
-ff 如果提供-o filename,则所有进程的跟踪结果输出到相应的filename.pid中,pid是各进程的进程号.
-F 尝试跟踪vfork调用.在-f时,vfork不被跟踪.
-h 输出简要的帮助信息.
-i 输出系统调用的入口指针.
-q 禁止输出关于脱离的消息.
-r 打印出相对时间关于,,每一个系统调用.
-t 在输出中的每一行前加上时间信息.
-tt 在输出中的每一行前加上时间信息,微秒级.
-ttt 微秒级输出,以秒了表示时间.
-T 显示每一调用所耗的时间.
-v 输出所有的系统调用.一些调用关于环境变量,状态,输入输出等调用由于使用频繁,默认不输出.
-V 输出strace的版本信息.
-x 以十六进制形式输出非标准字符串
-xx 所有字符串以十六进制形式输出.
-a column
设置返回值的输出位置.默认 为40.
-e expr
指定一个表达式,用来控制如何跟踪.格式如下:
[qualifier=][!]value1[,value2]...
qualifier只能是 trace,abbrev,verbose,raw,signal,read,write其中之一.value是用来限定的符号或数字.默认的 qualifier是 trace.感叹号是否定符号.例如:
-eopen等价于 -e trace=open,表示只跟踪open调用.而-etrace!=open表示跟踪除了open以外的其他调用.有两个特殊的符号 all 和 none.
注意有些shell使用!来执行历史记录里的命令,所以要使用\\.
-e trace=set
只跟踪指定的系统 调用.例如:-e trace=open,close,rean,write表示只跟踪这四个系统调用.默认的为set=all.
-e trace=file
只跟踪有关文件操作的系统调用.
-e trace=process
只跟踪有关进程控制的系统调用.
-e trace=network
跟踪与网络有关的所有系统调用.
-e strace=signal
跟踪所有与系统信号有关的 系统调用
-e trace=ipc
跟踪所有与进程通讯有关的系统调用
-e abbrev=set
设定 strace输出的系统调用的结果集.-v 等与 abbrev=none.默认为abbrev=all.
-e raw=set
将指 定的系统调用的参数以十六进制显示.
-e signal=set
指定跟踪的系统信号.默认为all.如 signal=!SIGIO(或者signal=!io),表示不跟踪SIGIO信号.
-e read=set
输出从指定文件中读出 的数据.例如:
-e read=3,5
-e write=set
输出写入到指定文件中的数据.
-o filename
将strace的输出写入文件filename
-p pid
跟踪指定的进程pid.
-s strsize
指定输出的字符串的最大长度.默认为32.文件名一直全部输出.
-u username
以username 的UID和GID执行被跟踪的命令

&nbsp;

&nbsp;

在调试的时候，strace能帮助你追踪到一个程序所执行的系统调用。当你想知道程序和操作系统如何交互的时候，这是极其方便的，比如你想知道执行了哪些系统调用，并且以何种顺序执行。

这个简单而又强大的工具几乎在所有的Linux操作系统上可用，并且可被用来调试大量的程序。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/222958kvi5oata5awkq5en.gif" alt="" />
<h3 id="toc_1">命令用法</h3>
让我们看看strace命令如何追踪一个程序的执行情况。

最简单的形式，strace后面可以跟任何命令。它将列出许许多多的系统调用。一开始，我们并不能理解所有的输出，但是如果你正在寻找一些特殊的东西，那么你应该能从输出中发现它。

让我们来看看简单命令ls的系统调用跟踪情况。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ strace ls</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223014ib3pcwpywb63cp33.png" alt="Stracing ls command" /></p>
<p class="article_img_desc"><em>Stracing ls command</em></p>
这是strace命令输出的前几行。其他输出被截去了。
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223015eyry76zmu1rnrumh.png" alt="Strace write system call (ls)" /></p>
<p class="article_img_desc"><em>Strace write system call (ls)</em></p>
上面的输出部分展示了write系统调用，它把当前目录的列表输出到标准输出。

下面的图片展示了使用ls命令列出的目录内容（没有使用strace）。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ ls</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223016pgtrfsguabftgcgb.png" alt="ls command output" /></p>
<p class="article_img_desc"><em>ls command output</em></p>

<h3 id="toc_2">选项1 寻找被程序读取的配置文件</h3>
Strace 的用法之一（除了调试某些问题以外）是你能找到被一个程序读取的配置文件。例如，
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ strace php </span><span class="lit">2</span><span class="pun">&gt;&amp;</span><span class="lit">1</span> <span class="pun">|</span><span class="pln"> grep php</span><span class="pun">.</span><span class="pln">ini</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223017lm99a9jun9tjz9s4.png" alt="Strace config file read by program" /></p>
<p class="article_img_desc"><em>Strace config file read by program</em></p>

<h3 id="toc_3">选项2 跟踪指定的系统调用</h3>
strace命令的-e选项仅仅被用来展示特定的系统调用（例如，open，write等等）

让我们跟踪一下cat命令的‘open’系统调用。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ strace </span><span class="pun">-</span><span class="pln">e open cat dead</span><span class="pun">.</span><span class="pln">letter</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223018o1f2fqiyqxxyoedq.png" alt="Stracing specific system call (open here)" /></p>
<p class="article_img_desc"><em>Stracing specific system call (open here)</em></p>

<h3 id="toc_4">选项3 跟踪进程</h3>
strace不但能用在命令上，而且通过使用-p选项能用在运行的进程上。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ sudo strace </span><span class="pun">-</span><span class="pln">p </span><span class="lit">1846</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223019iwwbuu6zuzsh0xhp.png" alt="Strace a process" /></p>
<p class="article_img_desc"><em>Strace a process</em></p>

<h3 id="toc_5">选项4 strace的统计概要</h3>
它包括系统调用的概要，执行时间，错误等等。使用-c选项能够以一种整洁的方式展示：
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ strace </span><span class="pun">-</span><span class="pln">c ls</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223020ceze3kqrkje66kln.png" alt="Strace summary display" /></p>
<p class="article_img_desc"><em>Strace summary display</em></p>

<h3 id="toc_6">选项5 保存输出结果</h3>
通过使用-o选项可以把strace命令的输出结果保存到一个文件中。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ sudo strace </span><span class="pun">-</span><span class="pln">o process_strace </span><span class="pun">-</span><span class="pln">p </span><span class="lit">3229</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223021u6zpuo9o19cc8h61.png" alt="Strace a process" /></p>
<p class="article_img_desc"><em>Strace a process</em></p>
之所以以sudo来运行上面的命令，是为了防止用户ID与所查看进程的所有者ID不匹配的情况。
<h3>选项6 显示时间戳</h3>
使用-t选项，可以在每行的输出之前添加时间戳。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ strace </span><span class="pun">-</span><span class="pln">t ls</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223022w9z8m2bh8ehhzwei.png" alt="Timestamp before each output line" /></p>
<p class="article_img_desc"><em>Timestamp before each output line</em></p>

<h3 id="toc_8">选项7 更精细的时间戳</h3>
-tt选项可以展示微秒级别的时间戳。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ strace </span><span class="pun">-</span><span class="pln">tt ls</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223024c1lilqr8qnriwj3j.png" alt="Time - Microseconds" /></p>
<p class="article_img_desc"><em>Time - Microseconds</em></p>
-ttt也可以向上面那样展示微秒级的时间戳，但是它并不是打印当前时间，而是显示自从epoch（译注：1970年1月1日00:00:00 UTC）以来的所经过的秒数。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ strace </span><span class="pun">-</span><span class="pln">ttt ls</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223025ljck265ij26x644c.png" alt="Seconds since epoch" /></p>
<p class="article_img_desc"><em>Seconds since epoch</em></p>

<h3 id="toc_9">选项8 相对时间</h3>
-r选项展示系统调用之间的相对时间戳。
<ol class="linenums">
	<li class="L0"><span class="pln">raghu@raghu</span><span class="pun">-</span><span class="typ">Linoxide</span> <span class="pun">~</span><span class="pln"> $ strace </span><span class="pun">-</span><span class="pln">r ls</span></li>
</ol>
<p class="article_img"><img src="https://dn-linuxcn.qbox.me/data/attachment/album/201409/30/223026j9lvfabf7ffphfsl.png" alt="Relative Timestamp" /></p>
<p class="article_img_desc"><em>Relative Timestamp</em></p>
