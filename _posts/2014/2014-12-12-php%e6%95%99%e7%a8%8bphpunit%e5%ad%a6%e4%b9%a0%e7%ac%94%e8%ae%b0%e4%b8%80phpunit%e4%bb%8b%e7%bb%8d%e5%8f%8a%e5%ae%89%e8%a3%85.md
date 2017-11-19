---
layout: post
title: PHP教程:PHPUnit学习笔记(一)PHPUnit介绍及安装
date: 2014-12-12 15:21
author: admin
comments: true
categories: []
---
最近学习并在项目中运用了PHPUnit做自动化测试，我将在博客上基于我的PHPUnit学习笔记进行连载，详细的介绍这个自动化测试框架。笔记内容基本上基于PHPUnit的官方文档和例子，里面加上我自己理解的翻译和配合描述代码。本笔记使用的PHPUnit版本为3.5.13, 测试平台为ubuntu10.10 PHP5.3.3
什么是PHPUnit？
PHPUnit是一个轻量级的PHP测试框架。它是在PHP5下面对JUnit3系列版本的完整移植，是xUnit测试框架家族的一员(它们都基于模式先锋Kent Beck的设计)
PHPUnit的安装
Linux各大发行版本基本上都带有phpunit的包，安装非常方便，例如ubuntu下直接运行下列命令即可安装好phpunit
sudo apt-get install phpunit
当然，如果你是一个喜欢折腾的Geek，需要其他的安装方式或者觉得默认包里面的版本不够新，那么请在你的linux上装好php的pear库，然后从下面Windows安装的第八个步骤开始安装最新版本phpunit吧
我重点讲一下windows下面phpunit的安装流程，相对linux，windows的要麻烦的多
1. 按照常规下载php的zip包和配置好php.ini，这里的例子使用的是C:\php-5.2.17-Win32
2. 把你的php目录加入系统环境变量path中，重启系统
点击查看原图
2. 开始 运行 输入 cmd，然后切换到你的php目录，我当前的就是C:\php-5.2.17-Win32
3. 输入go-pear.bat
首先脚本会询问是把pear安装为系统范围的还是本地拷贝，这里我们默认选择系统，直接回车即可
点击查看原图
4. 这时显示当前的路径配置，并询问你是否修改，我们保持默认依然回车即可，回车后脚本就会开始自动安装pear库了
点击查看原图
5. 安装的时候脚本会提示你设定php.ini的里面include_path，我们按照要求在php.ini里面设置好，设置好后回车即可
点击查看原图
6. 最后脚本会提醒你导入pear的系统变量注册文件，这个文件就在你的php目录中
点击查看原图
7. 输入回车，pear的安装就完成了, 测试pear是否装好，可以直接在命令行输入pear，如果你看到下列的输出，那就是ok了
点击查看原图
8. 开始安装phpunit，首先升级pear，输入命令
pear upgrade pear
9. 再依次输入下列命令添加pear的频道, 添加的时候可能会因为网络问题可能会提示失败，多试几次即可
pear channel-discover components.ez.no 

pear channel-discover pear.phpunit.de

pear channel-discover pear.symfony-project.com
小提示:
添加时如果出现下列错误提示，请在php.ini里面开启 php_openssl.dll 这个扩展
Unable to find the socket transport "ssl" - did you forget to enable it when you configured PHP?
10. 输入下列命令开始安装phpunit，同样，安装的时候因为网络问题可能会提示失败，多试几次即可
pear install --alldeps --force pear.phpunit.de/PHPUnit
pear install pear.phpunit.de/DbUnit
11. 待命令运行完毕后，phpunit就安装好了，我们可以通过输入phpunit来测试是否安装成功
点击查看原图
如果你输入php出现上图的显示，那么你的phpunit就安装完成了。
笔记到此结束，下篇将介绍phpunit的基本用法
