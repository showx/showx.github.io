---
layout: post
title: 图解autoscan、aclocal、autoheader、automake、autoconf、configure、make
date: 2010-03-12 09:16
author: admin
comments: true
categories: []
---
1.autoscan (autoconf): 扫描源代码以搜寻普通的可移植性问题，比如检查编译器，库，头文件等，生成文件configure.scan,它是configure.ac的一个雏形。

??? your source files --&gt; [autoscan*] --&gt; [configure.scan] --&gt; configure.ac

2.aclocal (automake):根据已经安装的宏，用户定义宏和acinclude.m4文件中的宏将configure.ac文件所需要的宏集中定义到文件 aclocal.m4中。aclocal是一个perl 脚本程序，它的定义是：“aclocal - create aclocal.m4 by scanning configure.ac”
<pre>user input files   optional input     process          output files
================   ==============     =======          ============

                    acinclude.m4 - - - - -.
                                          V
                                      .-------,
configure.ac ------------------------&gt;|aclocal|
                 {user macro files} -&gt;|       |------&gt; aclocal.m4
                                      `-------'
3.autoheader(autoconf): 根据configure.ac中的某些宏，比如cpp宏定义，运行m4，声称config.h.in

user input files    optional input     process          output files
================    ==============     =======          ============

                    aclocal.m4 - - - - - - - .
                                             |
                                             V
                                     .----------,
configure.ac -----------------------&gt;|autoheader|----&gt; autoconfig.h.in
                                     `----------'</pre>
4.automake: automake将Makefile.am中定义的结构建立Makefile.in，然后configure脚本将生成的Makefile.in文件转换为Makefile。如果在configure.ac中定义了一些特殊的宏，比如AC_PROG_LIBTOOL，它会调用libtoolize，否则它会自己产生config.guess和config.sub
<pre>user input files   optional input   processes          output files
================   ==============   =========          ============

                                     .--------,
                                     |        | - - -&gt; COPYING
                                     |        | - - -&gt; INSTALL
                                     |        |------&gt; install-sh
                                     |        |------&gt; missing
                                     |automake|------&gt; mkinstalldirs
configure.ac -----------------------&gt;|        |
Makefile.am  -----------------------&gt;|        |------&gt; Makefile.in
                                     |        |------&gt; stamp-h.in
                                 .---+        | - - -&gt; config.guess
                                 |   |        | - - -&gt; config.sub
                                 |   `------+-'
                                 |          | - - - -&gt; config.guess
                                 |libtoolize| - - - -&gt; config.sub
                                 |          |--------&gt; ltmain.sh
                                 |          |--------&gt; ltconfig
                                 `----------'</pre>
5.autoconf:将configure.ac中的宏展开，生成configure脚本。这个过程可能要用到aclocal.m4中定义的宏。
<pre>user input files   optional input   processes          output files
================   ==============   =========          ============

aclocal.m4 ,autoconfig.h.in - - - - - - -.
                                         V
                                     .--------,
configure.ac -----------------------&gt;|autoconf|------&gt; configure</pre>
<pre></pre>
<pre>6. ./configure的过程</pre>
<pre>
                                           .-------------&gt; [config.cache]
     configure* --------------------------+-------------&gt; config.log
                                          |
              [config.h.in] -.            v            .--&gt; [autoconfig.h]
                             +-------&gt; config.status* -+                   
              Makefile.in ---'                         `--&gt;   Makefile</pre>
<pre></pre>
<pre>7. make过程</pre>
<pre></pre>
<pre>[autoconfig.h] -.
                     +--&gt; make* ---&gt;  程序
        Makefile   ---'</pre>
<pre></pre>
<pre>.---------,
                   config.site - - -&gt;|         |
                  config.cache - - -&gt;|<strong><span style="text-decoration: underline;">configure</span></strong>| - - -&gt; config.cache
                                     |         +-,
                                     `-+-------' |
                                       |         |----&gt; config.status
                   config.h.in -------&gt;|config-  |----&gt; config.h
                   Makefile.in -------&gt;|  .status|----&gt; Makefile
                                       |         |----&gt; stamp-h
                                       |         +--,
                                     .-+         |  |
                                     | `------+--'  |
                   ltmain.sh -------&gt;|ltconfig|-------&gt; libtool
                                     |        |     |
                                     `-+------'     |
                                       |config.guess|
                                       | config.sub |
                                       `------------'
?

<pre>.--------,
                   Makefile ------&gt;|        |
                   config.h ------&gt;|  <strong><span style="text-decoration: underline;">make</span></strong>  |
{project sources} ----------------&gt;|        |--------&gt; {project targets}
                                 .-+        +--,
                                 | `--------'  |
                                 |   libtool   |
                                 |   missing   |
                                 |  install-sh |
                                 |mkinstalldirs|
                                 `-------------'</pre>
</pre>
