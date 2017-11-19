---
layout: post
title: SublimeText自带格式化代码功能 - reindent
date: 2014-10-13 15:18
author: admin
comments: true
categories: []
---
刚刚找到的一个在SublimeText中格式化代码的方法，其实格式化代码这个功能是SublimeText本身就有的功能，只是一直没有被小觉发掘。

之前小觉对于格式化代码都是复制代码，然后粘贴到在线站长工具里面进行代码的格式化，但是在小觉测试了以下SublimeText自带的格式化代码功能之后，小觉认为这已经是个多余的步骤了。

SublimeText自带格式化代码功能 - reindent

那么，说到这里，SublimeText自带格式化代码功能应该怎么使用呢？

这个功能被SublimeText命名为reindent，如果你使用了SublimeText汉化包的话叫做“再次缩进”，但是这种叫法说不通。

该选项的路径：Edit - Line - Reindent（中文路径则是：编辑 - 行 - 再次缩进）

同时说明一下，该功能并不需要选中代码之后才能执行格式化功能，其默认是格式化整个文件里的代码。

接下来就说到主题了，应该如何对该格式化代码功能进行快捷键组合的设置呢？

1、首先通过以下路径打开用户按键绑定文件：

Preferences → Key Bindings – User

2、然后在其中添加以下代码（如果你有需要的话，其中的快捷键组合是可以自己定义的）：

{"keys": ["ctrl+shift+r"], "command": "reindent" , "args": {"single_line": false}}

在这儿请注意每组快捷键组合包含着一个中括号里面，通过大括号定义一组快捷键，然后通过英文逗号进行分隔，具体可参考下图：

SublimeText自带格式化代码功能 - reindent

本文到这儿就结束了吗？不，下面说下如果SublimeText自带的格式化代码不适合用在你所使用的语言（比如SQL、Ruby等）的话，你可以通过插件的方式进行配置，具体请看下述操作：

1、以下内容基于已经你已经在你的SublimeText中安装了package control（教程在本站有）；

2、通过快捷键组合ctrl+shift+P唤出命令面板

3、在面板中输入“install package”后回车

4、接着输入“format”（即格式化的意思），在弹出的列表中找到对应你所想要进行格式化操作的语言，具体看图：

SublimeText自带格式化代码功能 - reindent

那么到这儿就真的结束了，不过，很快就有下一篇文章，敬请期待。
