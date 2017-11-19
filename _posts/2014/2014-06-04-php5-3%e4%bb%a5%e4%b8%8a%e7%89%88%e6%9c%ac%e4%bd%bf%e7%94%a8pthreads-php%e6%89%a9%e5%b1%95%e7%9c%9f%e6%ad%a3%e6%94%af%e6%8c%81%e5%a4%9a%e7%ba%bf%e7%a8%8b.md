---
layout: post
title: PHP5.3以上版本使用pthreads PHP扩展真正支持多线程
date: 2014-06-04 15:59
author: admin
comments: true
categories: []
---
原标题：PHP 真正多线程的使用

PHP 5.3 以上版本，使用pthreads PHP扩展，可以使PHP真正地支持多线程。多线程在处理重复性的循环任务，能够大大缩短程序执行时间。

我之前的文章中说过，大多数网站的性能瓶颈不在PHP服务器上，因为它可以简单地通过横向增加服务器或CPU核数来轻松应对（对于各种云主机，增加VPS或CPU核数就更方便了，直接以备份镜像增加VPS，连操作系统、环境都不用安装配置），而是在于MySQL数据库。如果用 MySQL 数据库，一条联合查询的SQL，也许就可以处理完业务逻辑，但是，遇到大量并发请求，就歇菜了。如果用 NoSQL 数据库，也许需要十次查询，才能处理完同样地业务逻辑，但每次查询都比 MySQL 要快，十次循环NoSQL查询也许比一次MySQL联合查询更快，应对几万次/秒的查询完全没问题。如果加上PHP多线程，通过十个线程同时查询NoSQL，返回结果汇总输出，速度就要更快了。我们实际的APP产品中，调用一个通过用户喜好实时推荐商品的PHP接口，PHP需要对BigSea NoSQL数据库发起500~1000次查询，来实时算出用户的个性喜好商品数据，PHP多线程的作用非常明显。

PHP扩展下载：https://github.com/krakjoe/pthreads
PHP手册文档：http://php.net/manual/zh/book.pthreads.php
1、扩展的编译安装(Linux），编辑参数 --enable-maintainer-zts 是必选项：

cd /Data/tgz/php-5.5.1
./configure --prefix=/Data/apps/php --with-config-file-path=/Data/apps/php/etc --with-mysql=/Data/apps/mysql --with-mysqli=/Data/apps/mysql/bin/mysql_config --with-iconv-dir --with-freetype-dir=/Data/apps/libs --with-jpeg-dir=/Data/apps/libs --with-png-dir=/Data/apps/libs --with-zlib --with-libxml-dir=/usr --enable-xml --disable-rpath --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --with-curl --enable-mbregex --enable-fpm --enable-mbstring --with-mcrypt=/Data/apps/libs --with-gd --enable-gd-native-ttf --with-openssl --with-mhash --enable-pcntl --enable-sockets --with-xmlrpc --enable-zip --enable-soap --enable-opcache --with-pdo-mysql --enable-maintainer-zts
make clean
make
make install        
 
unzip pthreads-master.zip
cd pthreads-master
/Data/apps/php/bin/phpize
./configure --with-php-config=/Data/apps/php/bin/php-config
make
make install

vi /Data/apps/php/etc/php.ini
添加：

extension = "pthreads.so"
2、给出一段PHP多线程、与For循环，抓取百度搜索页面的PHP代码示例：

<?php
  class test_thread_run extends Thread 
  {
      public $url;
      public $data;
 
      public function __construct($url)
      {
          $this->url = $url;
      }
 
      public function run()
      {
          if(($url = $this->url))
          {
              $this->data = model_http_curl_get($url);
          }
      }
  }
 
  function model_thread_result_get($urls_array) 
  {
      foreach ($urls_array as $key => $value) 
      {
          $thread_array[$key] = new test_thread_run($value["url"]);
          $thread_array[$key]->start();
      }
 
      foreach ($thread_array as $thread_array_key => $thread_array_value) 
      {
          while($thread_array[$thread_array_key]->isRunning())
          {
              usleep(10);
          }
          if($thread_array[$thread_array_key]->join())
          {
              $variable_data[$thread_array_key] = $thread_array[$thread_array_key]->data;
          }
      }
      return $variable_data;
  }
 
  function model_http_curl_get($url,$userAgent="") 
  {
      $userAgent = $userAgent ? $userAgent : 'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.2)'; 
      $curl = curl_init();
      curl_setopt($curl, CURLOPT_URL, $url);
      curl_setopt($curl, CURLOPT_RETURNTRANSFER, 1);
      curl_setopt($curl, CURLOPT_TIMEOUT, 5);
      curl_setopt($curl, CURLOPT_USERAGENT, $userAgent);
      $result = curl_exec($curl);
      curl_close($curl);
      return $result;
  }
 
  for ($i=0; $i < 100; $i++) 
  { 
      $urls_array[] = array("name" => "baidu", "url" => "http://www.baidu.com/s?wd=".mt_rand(10000,20000));
  }
 
  $t = microtime(true);
  $result = model_thread_result_get($urls_array);
  $e = microtime(true);
  echo "多线程：".($e-$t)."
";
 
  $t = microtime(true);
  foreach ($urls_array as $key => $value) 
  {
      $result_new[$key] = model_http_curl_get($value["url"]);
  }
  $e = microtime(true);
  echo "For循环：".($e-$t)."
";
?>

================
 pthreads 安装error: pthreads requires ZTS,please re-compile PHP with ZTS enabled
研发的同事要求安装php pthread扩展！
   tar zxvf pthreads-0.0.44.tgz 
   cd pthreads-0.0.44
 
   /usr/local/webserver/php/bin/phpize 
   ./configure  --with-php-config=/usr/local/webserver/php/bin/php-config 
##但在configure时却出错，提示：
configure: error: pthreads requires ZTS, please re-compile PHP with ZTS enabled
原因： 我在编译php的时候没有加入 --enable-maintainer-zts   ，这个必须要重新编译php，不能动态加载的！
于是我重新编译了php，在原来的编译参数基础上那个加入了  --enable-maintainer-zts ，重新编译安装php即可！
   make
   make install
并在php.ini中加入：
extension_dir = "/usr/local/webserver/php/lib/php/extensions/no-debug-zts-20100525"  ##必须和你的目录相对应！
extension =pthreads.so
重启php服务！/etc/init.d/php-fpm restart
