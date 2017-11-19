---
layout: post
title: mysql中Table is read only的解决方法小结
date: 2014-07-10 16:41
author: admin
comments: true
categories: []
---
如果是使用中的数据库突然出现些类问题 
在Linux下面执行下面命令就可以了，当然你要找到你的mysql目录 

linux中 
复制代码 代码如下:

/usr/local/mysql/bin/mysqladmin -u root -p flush-tables 

windows中 
可以在cmd中执行lush-tables 
也可以在phpmyadmin 直利用修复表进行修改 

如果是导入还原数据 
，所以将该数据库文件夹下面所有表文件chmod成777，chown成”_mysql”，但这次问题更严重，drupal里面现实table crached。没办法，马上Google，发现其实解决起来挺容易的。 
首先，找到mysqladmin所在位置，一般都在mysql/bin下面，然后运行一下命令： 
复制代码 代码如下:

./mysqladmin -u root -p flush-tables 

之后输入root账号的密码，马上就好了，没有任何任何提示，重新打开drupal，一切正常。 
通过这次，也找到了数据库文件的正确权限设置：data下面数据库文件夹700，表文件660，所有文件都应owned by mysql。

以下也是从window数据库转移到linux服务器出的问题

一个Discuz论坛，原来架在windows下，用的是GBK编码，MYSQL版本是5.0的。 
现在需要转移到Linux下，我本来建议用mysqldump导出的方法，但同事希望直接用data目录下的数据库目录。 

那就先用移目录的方式试下，在新服务器创建数据库，然后将旧的目录移过来。 

在mysql中，Select之类的都正常，但在网页程序中提示：Table 'cdb_posts' is read only 

给数据库目录的所属用户和组改为mysql，并加上777的权限，还是一样提示。 

程序中使用root连接，也是一样的提示。 

想用myisamchk来检查一下，也提示read only。 

最终在这里找到了解决方法：http://www.mysqltalk.org/re-the-table-is-read-only-vt154092.html 

引用 
I just encountered a similar problem on one of my production servers 
this morning. (I'm still investigating the cause.) After doing a 
quick bit of Google-searching, this solved my problem: 

mysqladmin -u <username> -p flush-tables 

By the way: All directories in /var/lib/mysql should have 700 
permissions (owned my the mysql user) and everything within those 
directories should be 660 (owned by the mysql user and mysql group). 

运行flush-tables后，read only问题解决：） 

然后发现数据结构和内容还是有问题，用myisamchk查错无效，后来用mysqldump导，不过也还是碰到了一大堆问题，由于要转的数据库挺大，化了很长时间，最终没有继续下去。 
了解了一些知识点，记录一下： 

就是mysql5导出的有default-charact的设置，mysql4不支持，需要加skip-opt参数，如： 
mysqldump -uroot -p --default-character-set=gbk -skip-opt databse > hx.sql 
参考文章：Mysql 数据库字符集转换 

最后找了台mysql5的服务器，用mysqldump导出，mysql导入，一次成功！ 
发现用mysqldump导出一个表，300w多条记录，用了才4分多钟，每秒处理1w多记录，快啊！导入时，差不多用了十几分钟，每秒导入几千条也很快了：）
