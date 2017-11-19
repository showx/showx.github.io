---
layout: post
title: mysql安全启动脚本mysqld_safe详细介绍
date: 2014-12-17 16:42
author: admin
comments: true
categories: []
---
这篇文章主要介绍了mysql安全启动脚本mysqld_safe详细介绍,mysqld_safe增加了一些安全特性,需要的朋友可以参考下

 

在Unix和NetWare中推荐使用mysqld_safe来启动mysqld服务器。mysqld_safe增加了一些安全特性，例如当出现错误时重启服务器并向错误日志文件写入运行时间信息。本节后面列出了NetWare的特定行为。
　　注释：为了保持同旧版本MySQL的向后兼容性，MySQL二进制分发版仍然包括safe_mysqld作为mysqld_safe的符号链接。但是，你不应再依赖它，因为再将来将删掉它。
　　默认情况下，mysqld_safe尝试启动可执行mysqld-max（如果存在），否则启动mysqld。该行为的含义是：
　　· 在Linux中，MySQL-Max RPM依赖该mysqld_safe的行为。RPM安装可执行mysqld-max，使mysqld_safe从该点起自动使用可执行命令。
　　· 如果你安装包括mysqld-max服务器的MySQL-Max分发版，后面升级到非-Max的MySQL版本，mysqld_safe仍然试图运行旧的 mysqld-max服务器。升级时，你应手动删除旧的mysqld-max服务器以确保mysqld_safe运行新的mysqld服务器。
　　要想越过默认行为并显式指定你想要运行哪个服务器，为mysqld_safe指定--mysqld或--mysqld-version选项。
　　mysqld_safe从选项文件的[mysqld]、[server]和 [mysqld_safe]部分读取所有选项。为了保证向后兼容性，它还读取 [safe_mysqld]部分，尽管在MySQL 5.1安装中你应将这部分重新命名为[mysqld_safe]。
　　mysqld_safe支持下面的选项：
　　· --help
　　显示帮助消息并退出。
　　· --autoclose
　　(只在NetWare中)在NetWare中，mysqld_safe可以保持窗口。当你关掉mysqld_safe NLM时，窗口不按默认设置消失。相反，它提示用户输入：
　　*<NLM has terminated; Press any key to close the screen>*如果你想让NetWare自动关闭窗口，在mysqld_safe中使用--autoclose选项。
　　· --basedir=path
　　MySQL安装目录的路径。
　　· --core-file-size=size
　　mysqld能够创建的内核文件的大小。选项值传递给ulimit -c。
　　· --datadir=path
　　数据目录的路径。
　　· --defaults-extra-file=path
　　除了通用选项文件所读取的选项文件名。如果给出，必须首选该选项。
　　· --defaults-file=path
　　读取的代替通用选项文件的选项文件名。如果给出，必须首选该选项。
　　· --ledir=path
　　包含mysqld程序的目录的路径。使用该选项来显式表示服务器位置。
　　· --log-error=path
　　将错误日志写入给定的文件。参见5.11.1节，“错误日志”。
　　· --mysqld=prog_name
　　想要启动的服务器程序名(在ledir目录)。如果你使用MySQL二进制分发版但有二进制分发版之外的数据目录需要该选项。
　　· --mysqld-version =suffix
　　该选项类似--mysqld选项，但你只指定服务器程序名的后缀。基本名假定为mysqld。 例如，如果你使用--mysqld-version =max，mysqld_safe启动ledir目录中的mysqld-max程序。如果--mysqld-version的参数为 空，mysqld_safe使用目录中的mysqld。
　　· --nice=priority
　　使用nice程序根据给定值来设置服务器的调度优先级。
　　· --no-defaults
　　不要读任何选项文件。如果给出，必须首选该选项。
　　· --open-files-limit=count
　　mysqld能够打开的文件的数量。选项值传递给 ulimit -n。请注意你需要用root启动mysqld_safe来保证正确工作！
　　· --pid-file=path
　　进程ID文件的路径。
　　· --port=port_num
　　用来帧听TCP/IP连接的端口号。端口号必须为1024或更大值，除非MySQL以root系统用户运行。
　　· --skip-character-set-client-handshake
　　忽略客户端发送的字符集信息，使用服务器的默认字符集。(选择该选项，MySQL的动作与MySQL 4.0相同）。 
　　· --socket=path
　　用于本地连接的Unix套接字文件。
　　· --timezone=zone
　　为给定的选项值设置TZ时区环境变量。从操作系统文档查阅合法的时区规定格式。
　　· --user={user_name | user_id}
　　以用户名user_name或数字用户ID user_id运行mysqld服务器。(本文中的“用户”指系统登录账户，而不是 授权表中的MySQL用户）。
　　执行mysqld_safe时，必须先给出--defaults-file或--defaults-extra-option，或不使用选项文件。例如，该命令将不使用选项文件：
　　mysqld_safe --port=port_num --defaults-file=file_name相反，使用下面的命令：
　　mysqld_safe --defaults-file=file_name --port=port_num一般情况mysqld_safe脚本可以启动从源码或二进制MySQL分发版安装的服务器，即使这些分发版将服务器安装到 稍微不同的位置。(参见2.1.5节，“安装布局”）。 mysqld_safe期望下面的其中一个条件是真的：
　　· 可以根据调用mysqld_safe的目录找到服务器和数据库。在二进制分发版中，mysqld_safe看上去在bin和data目录的工作目录下。对 于源码分发版，为libexec和var目录。如果你从MySQL安装目录执行mysqld_safe应满足该条件(例如，二进制分发版为/usr /local/mysql)。
　　· 如果不能根据工作目录找到服务器和数据库，mysqld_safe试图通过绝对路径对它们定位。典型位置为/usr/local/libexec和 /usr/local/var。实际位置由构建分发版时配置的值确定如果MySQL安装到配置时指定的位置，它们应该是正确的。
　　因为mysqld_safe试图通过工作目录找到服务器和数据库，只要你从MySQL安装目录运行mysqld_safe，可以将MySQL二进制分发版安装到其它位置：
　　shell> cd mysql_installation_directoryshell> bin/mysqld_safe &如果mysqld_safe失败，即使从MySQL安装目录调用仍然失败，你可以指定--ledir和--datadir选项来指示服务器和数 据库在你的系统中的安装目录。
　　一般情况，你不应编辑mysqld_safe脚本。相反，应使用命令行选项或my.cnf选项 文件的[mysqld_safe]部分的选项来配置mysqld_safe。一般不需要编辑mysqld_safe来正确启动服务器。但是，如果你编辑， 将来升级MySQL后会覆盖你修改的mysqld_safe版本，因此你应对你修改的版本进行备份以便将来重装。
　　在NetWare中，mysqld_safe是一个NetWare Loadable Module (NLM)，从原Unix shell脚本移植。它执行：
　　1. 检查系统和选项。
　　2. 检查MyISAM表。
　　3. 保持MySQL服务器窗口。
　　4. 启动并监视mysqld，如果因错误终止则重启。
　　5. 将mysqld的错误消息发送到数据目录中的host_name.err 文件。
　　6. 将mysqld_safe的屏幕输出发送到数据目录中的host_name.safe文件
