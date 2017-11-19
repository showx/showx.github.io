---
layout: post
title: 使用 Zend Opcache 加速 PHP
date: 2015-03-16 15:18
author: admin
comments: true
categories: []
---
Optimizer+ 是 Zend 开发的闭源但可以免费使用的 PHP 优化加速组件，是第一个也是最快的 opcode 缓存工具。现在，Zend 科技公司将 Optimizer+ 在 PHP License 下开源成为 Zend Opcache。

Zend OPcache 通过 opcode 缓存和优化提供更快的 PHP 执行过程。它将预编译的脚本文件存储在共享内存中供以后使用，从而避免了从磁盘读取代码并进行编译的时间消耗。同时，它还应用了一些代码优化模式，使得代码执行更快。

1. 什么是 opcode 缓存？¶

当解释器完成对脚本代码的分析后，便将它们生成可以直接运行的中间代码，也称为操作码（Operate Code，opcode）。Opcode cache 的目地是避免重复编译，减少 CPU 和内存开销。如果动态内容的性能瓶颈不在于 CPU 和内存，而在于 I/O 操作，比如数据库查询带来的磁盘 I/O 开销，那么 opcode cache 的性能提升是非常有限的。但是既然 opcode cache 能带来 CPU 和内存开销的降低，这总归是好事 —— 本着环保的态度，也应该尽量减少消耗不是？ :D

现代操作码缓存器（Optimizer+，APC2.0+，其他）使用共享内存进行存储，并且可以直接从中执行文件，而不用在执行前“反序列化”代码。这将带来显着的性能加速，通常降低了整体服务器的内存消耗，而且很少有缺点。

2. Optimizer+ 与 APC 的优缺点对比¶

Optimizer+ 于 2013年3月中旬改名为 Opcache。

根据 PHP wiki 上的讨论，Zend Opcache 即将整合到 php 5.5 中。作为 APC 的竞争对手，新生的 Zend Opcache 很有可能取代 APC 的位置，虽然 OptimizerPlus 没有象 APC 那样的 user cache 功能。

OPTIMIZER+ 相对 APC 的优点¶
性能。根据测试，Zend Optimizer+ 始终优于 APC。随代码差异，每秒钟处理的请求数高 5~20%。Google doc 上记录的测试结果中，WordPress 2.1.1（不知道为什么不用个新版本的 WP 来测试），性能提高约 8%。理论上来说，对于 WP 3.5.1，性能应该也能得到大约 5~10% 的提升吧。对于运行 WordPress 的服务器而言，使用 Optimizer+ 可以显著降低 CPU 使用率和提高页面加载速度（graphics here）。
支持新的 PHP 版本。Zend 和 PHP 社区都会帮助 Optimizer+ 能够支持最新版本的 PHP。
可靠性。Optimizer+ 拥有可选的损坏检测能力，可以防止因数据损坏而导致的服务器崩溃。
更好的兼容性。PHP 社区打算让 Optimizer+ 与社区支持的所有 PHP 版本相兼容。
APC 相对 OPTIMIZER+ 的优势¶
APC 有数据缓存 API，而 Optimizer+ 没有。
APC 能够回收旧的无效的脚本占用的内存。APC 有内存管理器，可以将那些不再使用的脚本关联的内存进行回收。而 Optimizer+ 不同，它将这样的内存标记为“脏的”，但并不会回收它们。一旦“脏的”内存占用配置阈值的百分比达到一定值，Optimizer+ 就将自己重新启动。这种行为在稳定性上既有优势也有劣势。
3. 使用 Zend Opcode¶

现在已经可以使用 Zend Opcache 替代 APC 作为 PHP 优化加速工具了。目前的 Zend Opcode 兼容 PHP 5.2.*、5.3.*、5.4.* 和 PHP-5.5 开发版。不过，将来会取消对 PHP 5.2 的支持。

注意：Zend Opcache 与 eaccelerator 相冲突。要安装 Zend Opcache，可能需要先卸载 eaccelerator —— 如果你用了这个加速模块的话。

从源码安装并配置¶
Zend Opcache 的源代码托管在 github 上，目前还是叫做 ZendOptimizerPlus。

安装步骤详见其 README 文件。

注意：

最好在本地虚拟机里测试之后再部署到自己的服务器上；
安装前最好先删除 eacceleratro、xcache 或 apc 等组件。
顺便说一句，从源码编译安装时需要用到 php-devel。README 中快速安装一节的开头就用到，

$PHP_DIR/bin/phpize
如果不清楚 phpize 的路径，可以，

whereis phpize
README 文件中也有相应的推荐优化设置。

从 EPEL 源安装并配置¶
我不喜欢从源码编译安装程序，一个是水平有限，一个就是怕麻烦。下面介绍从 EPEL 安装源安装 Zend Opcache，以 CentOS 上的操作为例，基于我的 VPS 的配置。

EPEL 社区已经提供了 Zend Opcache 的安装包，可以直接 yum 安装。当然，前提是已经配置使用了 EPEL 的安装源。如果没有，可以参考这里。

提醒一下，REMI 安装源上的 PHP 已经是 5.4 版本了。鉴于有人测试说 WordPress 在 PHP 5.4 上的性能要优于在 PHP 5.3 上的性能（10% faster and lower ram consuming），顺便升级一下 PHP 也不是什么坏事。

操作步骤：

配置使用 epel 安装源。已有则跳过。
删除 eaccelerator、xcache、apc：
yum remove php-eaccelerator php-xcache php-apcu
没有使用则跳过。

对系统执行升级：
yum update
目的是根据 remi 安装源的状态升级当前的 php 等软件到 remi 支持的最新版本。此时，可以看到系统有类似下面的输出：

Updating   : php-common-5.4.14-1.el6.remi.i686                                                         1/26

WARNING : These php-* RPM are not official Fedora / Red Hat build and
overrides the official ones. Don't file bugs on Fedora Project nor Red Hat.

Use dedicated forums http://forums.famillecollet.com/

warning: /etc/php.ini created as /etc/php.ini.rpmnew
  Updating   : mysql-libs-5.5.31-1.el6.remi.i686                                                         2/26

WARNING : This MySQL RPM is not an official Fedora / Red Hat build and it
overrides the official one. Don't file bugs on Fedora Project nor Red Hat.
Use dedicated forums http://forums.famillecollet.com/

warning: /etc/my.cnf created as /etc/my.cnf.rpmnew
表示我们现在要从 Fedora / Red Hat 的版本迁移到 Remi 版本了，所以不要去 Fedora / Red Hat 寻求帮助了。呵呵，貌似出问题都是在网上找，还真是很少到官方论坛里提问。像我这样的入门级用户，也不会遇到那么深度的问题。

安装 Zend Opcache（pecl 版本）：
yum install php-pecl-zendopcache
安装时产生的 opcache 的配置文件位于默认的 /etc/php.d 目录中：

opcache-default.blacklist
opcache.ini
这个配置文件采用的基本就是 README 中的推荐设置，只有几个地方需要修改。

vi /etc/php.d/opcache.ini
对照如下推荐配置修改并保存即可（可参考完整的 Zend Opcache 配置信息）：

opcache.memory_consumption=128
opcache.interned_strings_buffer=8
opcache.max_accelerated_files=4000
opcache.revalidate_freq=60
opcache.fast_shutdown=1
opcache.enable_cli=1
不需要修改 php.ini 配置，重起 Apache 服务使之生效：
service httpd restart
查询一下看看是否正确启动了：

php -v
输出结果类似于：

PHP 5.4.14 (cli) (built: Apr 11 2013 11:04:35)
Copyright (c) 1997-2013 The PHP Group
Zend Engine v2.4.0, Copyright (c) 1998-2013 Zend Technologies
    with Zend OPcache v7.0.1, Copyright (c) 1999-2013, by Zend Technologies
4. 体会¶

PHP 上有不少 opcode cache 组件，如 APC、eAccelerator、XCache 等。（参见 Wikipedia 上的 PHP accelerators 列表。）看 PHP wiki 上的意思，这个新引入的 Zend Opcache 的性能应该是最好的。不管用哪个组件，总归是用一个才好。

对于小型的服务器，似乎这几个组件的性能差异并不太明显。我的想法是，既然用了，那就用个最好的吧。但是如果你正在使用别的 opcode cache，比如上面提到的这几个中的一个，从性能提升上讲，倒是没必要立刻就换。

对这个站，首页生成时间：仅使用 PHP 的时候，大约 0.9s；使用 eAccelerator 大约 0.63s；使用 Zend Opcache 后大约 0.55s。测试得非常简陋，多打开几次看看 WP Super Cache 提供的页面生成时间，估计一个平均数。

登录到系统里看了看 Apache 进程的内存占用。之前一个进程不多大一会儿就能占用 40MB 以上的内存，现在基本上没有高于 40MB 的了。只是不知道是 php 5.4 的功劳呢，还是 Zend Opcache 的功劳。

不知道您觉得这个 Zend Opcache 的效果如何？如果您有兴趣，不妨留言写下您的测试结果。
