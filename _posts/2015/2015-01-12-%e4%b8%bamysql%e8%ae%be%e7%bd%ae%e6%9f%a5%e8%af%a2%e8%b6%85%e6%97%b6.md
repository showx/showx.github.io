---
layout: post
title: 为MySQL设置查询超时
date: 2015-01-12 22:46
author: admin
comments: true
categories: []
---
昨天有人在群里问, MySQL是否可以设置读写超时(非连接超时), 如果可以就可以避免一条SQL执行过慢, 导致PHP超时错误. 这个, 其实可以有. 只不过稍微要麻烦点.

首先, 在libmysql中, 是提供了MYSQL_OPT_READ_TIMEOUT设置项的, 并且libmysql中提供了设置相关设置项的API, mysql_options:

int STDCALL
mysql_options(MYSQL *mysql,enum mysql_option option, const void *arg)
{
  DBUG_ENTER("mysql_option");
  DBUG_PRINT("enter",("option: %d",(int) option));
  switch (option) {
  case MYSQL_OPT_CONNECT_TIMEOUT:
    mysql->options.connect_timeout= *(uint*) arg;
    break;
  /** 读超时时间 */
  case MYSQL_OPT_READ_TIMEOUT:
    mysql->options.read_timeout= *(uint*) arg;
    break;
  case MYSQL_OPT_WRITE_TIMEOUT:
    mysql->options.write_timeout= *(uint*) arg;
    break;
  case MYSQL_OPT_COMPRESS:
    mysql->options.compress= 1; 
 
   /* 以下省略 */
但是, 可惜的是, 目前只有mysqli扩展, 把mysql_options完全暴露给了PHP:

PHP_FUNCTION(mysqli_options)
{
 /** 有省略 */
     switch (Z_TYPE_PP(mysql_value)) {
        /** 没有任何限制, 直接传递给mysql_options */
        case IS_STRING:
            ret = mysql_options(mysql->mysql, mysql_option, Z_STRVAL_PP(mysql_value));
            break;
        default:
            convert_to_long_ex(mysql_value);
            l_value = Z_LVAL_PP(mysql_value);
            ret = mysql_options(mysql->mysql, mysql_option, (char *)&l_value);
            break;
    }
 
    RETURN_BOOL(!ret);
}
但是因为Mysqli并没有导出这个常量, 所以我们需要通过查看MySQL的代码, 得到MYSQL_OPT_READ_TIMEOUT的实际值, 然后直接调用mysql_options:

enum mysql_option
{
  MYSQL_OPT_CONNECT_TIMEOUT, MYSQL_OPT_COMPRESS, MYSQL_OPT_NAMED_PIPE,
  MYSQL_INIT_COMMAND, MYSQL_READ_DEFAULT_FILE, MYSQL_READ_DEFAULT_GROUP,
  MYSQL_SET_CHARSET_DIR, MYSQL_SET_CHARSET_NAME, MYSQL_OPT_LOCAL_INFILE,
  MYSQL_OPT_PROTOCOL, MYSQL_SHARED_MEMORY_BASE_NAME, MYSQL_OPT_READ_TIMEOUT,
  MYSQL_OPT_WRITE_TIMEOUT, MYSQL_OPT_USE_RESULT,
  MYSQL_OPT_USE_REMOTE_CONNECTION, MYSQL_OPT_USE_EMBEDDED_CONNECTION,
  MYSQL_OPT_GUESS_CONNECTION, MYSQL_SET_CLIENT_IP, MYSQL_SECURE_AUTH,
  MYSQL_REPORT_DATA_TRUNCATION, MYSQL_OPT_RECONNECT,
  MYSQL_OPT_SSL_VERIFY_SERVER_CERT
};
可以看到, MYSQL_OPT_READ_TIMEOUT为11.

现在, 我们就可以设置查询超时了:

<?php
$mysqli = mysqli_init();
$mysqli->options(11 /*MYSQL_OPT_READ_TIMEOUT*/, 1);
$mysql->real_connect(***);
不过, 因为在libmysql中有重试机制(尝试一次, 重试俩次), 所以, 最终我们设置的超时阈值都会三倍于我们设置的值.

也就是说, 如果我们设置了MYSQL_OPT_READ_TIMEOUT为1, 最终会在3s以后超时结束. 也就是说, 我们目前能设置的最短超时时, 就是3秒…

虽说大了点,, 不过总比没有好, 呵呵
