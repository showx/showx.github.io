---
layout: post
title: PHP CLI模式下的多进程应用［转］
date: 2015-01-12 22:45
author: admin
comments: true
categories: []
---
PHP在很多时候不适合做常驻的SHELL进程, 他没有专门的gc例程, 也没有有效的内存管理途径. 所以如果用PHP做常驻SHELL, 你会经常被内存耗尽导致abort而unhappy.

而且, 如果输入数据非法, 而脚本没有检测, 导致abort, 也会让你很不开心.

那? 怎么办呢?

多进程….

为什么呢?

 优点:
    1. 使用多进程, 子进程结束以后, 内核会负责回收资源
    2. 使用多进程,子进程异常退出不会导致整个进程Thread退出. 父进程还有机会重建流程.
    3. 一个常驻主进程, 只负责任务分发, 逻辑更清楚.
Then, 怎么做呢?

接下来, 我们使用PHP提供的POSIX和Pcntl系列函数, 来实现一个PHP命令解析器, 主进程负责接受用户输入, 然后fork子进程执行, 并负责回显子进程的结束状态.

代码如下, 我加了注释, 如果有不懂的地方, 可以翻阅手册相关函数, 或者回复留言.

#!/bin/env php
<?php
/** A example denoted muti-process application in php
 * @filename fork.php
 * @touch date Wed 10 Jun 2009 10:25:51 PM CST
 * @author Laruence<laruence@baidu.com>
 * @license http://www.zend.com/license/3_0.txt   PHP License 3.0
 * @version 1.0.0
*/
 
/** 确保这个函数只能运行在SHELL中 */
if (substr(php_sapi_name(), 0, 3) !== 'cli') {
    die("This Programe can only be run in CLI mode");
}
 
/**  关闭最大执行时间限制, 在CLI模式下, 这个语句其实不必要 */
set_time_limit(0);
 
$pid  = posix_getpid(); //取得主进程ID
$user = posix_getlogin(); //取得用户名
 
echo <<<EOD
USAGE: [command | expression]
input php code to execute by fork a new process
input quit to exit
 
        Shell Executor version 1.0.0 by laruence
EOD;
 
while (true) {
 
        $prompt = "\n{$user}$ ";
        $input  = readline($prompt);
 
        readline_add_history($input);
        if ($input == 'quit') {
               break;
          }
        process_execute($input . ';');
}
 
exit(0);
 
function process_execute($input) {
        $pid = pcntl_fork(); //创建子进程
        if ($pid == 0) {//子进程
                $pid = posix_getpid();
                echo "* Process {$pid} was created, and Executed:\n\n";
                eval($input); //解析命令
                exit;
        } else {//主进程
                $pid = pcntl_wait($status, WUNTRACED); //取得子进程结束状态
                if (pcntl_wifexited($status)) {
                        echo "\n\n* Sub process: {$pid} exited with {$status}";
                }
        }
}
  
但有一点, 我一定要提醒:

Process Control should not be enabled within a webserver environment and unexpected results may happen if any Process Control functions are used within a webserver environment.  --摘自PHP手册
也就是说, 打消你在PHP Web开发中使用多进程的念头吧!
