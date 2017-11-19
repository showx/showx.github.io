---
layout: post
title: vim之vundle
date: 2015-06-28 22:15
author: admin
comments: true
categories: []
---
Vundle(Vim bundle) 是一个vim的插件管理器。

其Github地址为: https://github.com/gmarik/vundle

如何使用Vundle  (个人使用环境为ubuntu 12.10)

1. 从Github下载vundle到本地:

$  git clone https://github.com/gmarik/vundle.git  ~/.vim/bundle/vundle

2. 配置vimrc文件:

如果是使用ubuntu apt安装的vim，其vimrc一般应该在/etc/vim/目录下

有些人的vimrc文件可能在个人工作目录下，为.vimrc文件

在vimrc文件中修改并按照下面例子加入相应需要的语句。

&nbsp;
<div class="dp-highlighter bg_html">
<div class="bar">
<div class="tools"><b>[html]</b> <a class="ViewSource" title="view plain" href="http://blog.csdn.net/codebistu/article/details/8257138#">view plain</a><a class="CopyToClipboard" title="copy" href="http://blog.csdn.net/codebistu/article/details/8257138#">copy</a>
<div><embed id="ZeroClipboardMovie_1" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_1"></embed></div>
</div>
</div>
<ol class="dp-xml" start="1">
	<li class="alt">set nocompatible               " be iMproved</li>
	<li class="">filetype off                   " required!       /**  从这行开始，vimrc配置 **/</li>
	<li class="alt"></li>
	<li class="">set rtp+=~/.vim/bundle/vundle/</li>
	<li class="alt">call vundle#rc()</li>
	<li class=""></li>
	<li class="alt">" let Vundle manage Vundle</li>
	<li class="">" required!</li>
	<li class="alt">Bundle 'gmarik/vundle'</li>
	<li class=""></li>
	<li class="alt">" My Bundles here:  /* 插件配置格式 */</li>
	<li class="">"</li>
	<li class="alt">" original repos on github （Github网站上非vim-scripts仓库的插件，按下面格式填写）</li>
	<li class="">Bundle 'tpope/vim-fugitive'</li>
	<li class="alt">Bundle 'Lokaltog/vim-easymotion'</li>
	<li class="">Bundle 'rstacruz/sparkup', {'rtp': 'vim/'}</li>
	<li class="alt">Bundle 'tpope/vim-rails.git'</li>
	<li class="">" vim-scripts repos  （vim-scripts仓库里的，按下面格式填写）</li>
	<li class="alt">Bundle 'L9'</li>
	<li class="">Bundle 'FuzzyFinder'</li>
	<li class="alt">" non github repos   (非上面两种情况的，按下面格式填写)</li>
	<li class="">Bundle 'git://git.wincent.com/command-t.git'</li>
	<li class="alt">" ...</li>
	<li class=""></li>
	<li class="alt">filetype plugin indent on     " required!   /** vimrc文件配置结束 **/</li>
	<li class="">"                                           /** vundle命令 **/</li>
	<li class="alt">" Brief help</li>
	<li class="">" :BundleList          - list configured bundles</li>
	<li class="alt">" :BundleInstall(!)    - install(update) bundles</li>
	<li class="">" :BundleSearch(!) foo - search(or refresh cache first) for foo</li>
	<li class="alt">" :BundleClean(!)      - confirm(or auto-approve) removal of unused bundles</li>
	<li class="">"</li>
	<li class="alt">" see :h vundle for more details or wiki for FAQ</li>
	<li class="">" NOTE: comments after Bundle command are not allowed..</li>
</ol>
</div>
注意，vundle会自动给你下载和管理插件，所以，你只要填上你所需要的插件名称即可。对于不同类型的插件，有不同的地址填写方法。按上面的方法填写完毕就可以了。

填写完成，保存退出后，打开一个vim窗口。在命令模式下输入

:BundleList    //会显示你vimrc里面填写的所有插件名称

:BundleInstall  //会自动下载安装或更新你的插件。

PS: <a href="https://github.com/vim-scripts">https://github.com/vim-scripts</a> 和 http://vim-scripts.org/vim/scripts.html 这两个网站上都是vim-scripts的插件，即你只需在vimrc中添加你想要的插件名称即可。

&nbsp;
