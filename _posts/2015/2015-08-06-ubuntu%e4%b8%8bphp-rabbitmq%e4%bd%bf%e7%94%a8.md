---
layout: post
title: Ubuntu下PHP + RabbitMQ使用
date: 2015-08-06 15:36
author: admin
comments: true
categories: []
---
RabbitMQ是一个开源的基于AMQP（Advanced Message Queuing Protocol）标准，并且可靠性高的企业级消息系统，目前很多网站在用，包括reddit,Poppen.de等。

1. Ubuntu下安装RabbitMQ
sudo apt-get install rabbitmq-server
sudo /etc/init.d/rabbitmq-server start

2. 安装librabbitmq
sudo apt-get install mercurial
hg clone http://hg.rabbitmq.com/rabbitmq-c
cd rabbitmq-c
hg clone http://hg.rabbitmq.com/rabbitmq-codegen codegen
autoreconf -i && ./configure && make && sudo make install

3. 安装php-rabbit扩展
wget http://php-rabbit.googlecode.com/files/php-rabbit.r91.tar.gz
tar -zxvf php-rabbit.r91.tar.gz
cd php-rabbit.r91
/path/to/php/bin/phpize
./configure –with-amqp –with-php-config=/path/to/php/bin/php-config
make && sudo make install
编辑 php.ini 添加：
extension=rabbit.so
输出phpinfo看下是否扩展已经加载成功,have fun:)

4. Demo程序
producer:

<?php
/**
 * producer demo
 *
 * @author wei
 * @version $Id$
 **/
$params = array('host' =>'localhost',
                'port' => 5672,
                'login' => 'guest',
                'password' => 'guest',
                'vhost' => '/');
$cnn = new AMQPConnect($params);
 
// declare Exchange
$exchange = new AMQPExchange($cnn);
$exchange->declare('ex1', 'topic', AMQP_DURABLE );
 
// declare Queue
$queue = new AMQPQueue($cnn);  
$queue->declare('queue1', AMQP_DURABLE); 
 
// bind Queue
$queue->bind('ex1','wei.#');
 
// publishing
$msg = "msg";
 
for ($i=0; $i < 100; $i++) { 
	$res = $exchange->publish($i . 'msg', 'wei.' . $i);
	if ($res) {
		echo $i . 'msg' . " Yes\n";
	} else {
		echo $i . 'msg' . " No\n";
	}
}
 
?>



MAC 上面安装比较容易可以直接使用 macport，包括 php 和 它的扩展，上面都有最新的版本

Linux 上面一般需要自己编译
注意:扩展是C写的,由于C与RabbitMQ通信一般需要依赖rabbitmq-c库(也就是librabbitmq),所以编译扩展前需要先装依赖库。不同版本的扩展，对php版本和librabbitmq兼容性不一样。下面这个版本是经过本人测试的，可以兼容的。
rabbitmq-c -0.4.1 ， amqp 扩展 1.4.0 ， php 5.5.9

一键安装脚本：
[cpp] view plaincopy在CODE上查看代码片派生到我的代码片
#!/bin/bash  
set -e  
  
#install cmake  
yum -y install cmake  
  
#download rabbitmq-c  
wget https://github.com/alanxz/rabbitmq-c/releases/download/v0.4.1/rabbitmq-c-0.4.1.tar.gz -O rabbitmq-c.tar.gz  
  
#extract tar.gz  
tar xvfz rabbitmq-c.tar.gz  
cd rabbitmq-c-0.4.1/  
  
#cmake and build  
mkdir build && cd build  
cmake ..  
cmake --build [--config Release] .  
  
#make and make install  
make && make install  
  
#install pecl php amqp 1.4.0 版本  
pecl install amqp  
  
#add php.ini  
echo "extension = amqp.so" >> /etc/php/conf.d/amqp.ini  
