---
layout: post
title: Ubuntu下pptp用freeradius和mysql进行用户管理
date: 2014-09-19 14:35
author: admin
comments: true
categories: []
---
Ubuntu下pptp用freeradius流量控制

1 前置条件

已经安装好apache2 mysql php5
PPTP能够正常工作。
此文在ubuntu12.04 LTS下验证通过。

2 安装FreeRADIUS

shell中输入以下命令，安装freeradius，freeradius-mysql, freeradius-utils模块。完装完成后会自动启动freeradius，第一次启动会生成密钥，稍等片刻即可。

1
sudo apt-get install freeradius freeradius-mysql freeradius-utils

安装完后，我们编辑/etc/freeradius/users，

1
sudo vim /etc/freeradius/users
查找 steve Cleartext (76-84行), 取消注释，用于添加一个steve的用户。

1
2
3
4
5
6
7
8
9
steve  Cleartext-Password := "testing"
       Service-Type = Framed-User,
       Framed-Protocol = PPP,
       Framed-IP-Address = 172.16.3.33,
       Framed-IP-Netmask = 255.255.255.0,
       Framed-Routing = Broadcast-Listen,
       Framed-Filter-Id = "std.ppp",
       Framed-MTU = 1500,
       Framed-Compression = Van-Jacobsen-TCP-IP
以DEBUG模式启动freeradius.

1
2
sudo service freeradius stop
sudo freeradius -X
大写-X，意思是以debug模式运行，可以让调试信息直接。
新开一个窗口执行以下命令

1
radtest steve testing localhost 0 testing123
如果看到新窗口中输出Access-Accept就说明连接成功了。
如果看到类似“Ignoring request to authentication address * port 1812 from unknownclient”的文字，可能需要去修改/etc/freeradius/clients.conf，将client localhost段下的ipaddr改为服务器的IP，而不是127.0.0.1。

测试连接成功后，我们可以把users里临时修改的内容恢复。

3 FreeRadius MySQL 模块配置

3.1 启用MySQL模块支持

编辑/etc/freeradius/radiusd.conf，查找”sql.conf”(683行)，去掉#号

1
sudo vim /etc/freeradius/radiusd.conf
3.2 创建 radius 数据库

1
mysqladmin -uroot -p123456 create radius;
把123456改成mysql的root密码

3.3 创建radius帐号的密码

1
2
3
cd /etc/freeradius/sql/mysql
sudo sed -i 's/radpass/123456/g' admin.sql
sudo sed -i 's/radpass/123456/g' /etc/freeradius/sql.conf
其中radpass为默认的radius的密码。请把123456修改为你需要的密码。

3.4 生成radius数据库中的数据表

由于此目录下几个sql文件权限设置为640，会导致mysql不能读取sql文件，所以先要设置sql文件的属性为644。

1
2
3
4
5
6
7
8
sudo chmod 755 /etc/freeradius/sql/mysql
sudo chmod 644 /etc/freeradius/sql/mysql/*.sql
mysql -uroot -p123456 < admin.sql
mysql -uroot -p123456 radius < cui.sql
mysql -uroot -p123456 radius < ippool.sql
mysql -uroot -p123456 radius < nas.sql
mysql -uroot -p123456 radius < schema.sql
mysql -uroot -p123456 radius < wimax.sql
3.5打开从数据库获取nas信息支持

freeradius默认从 “/etc/freeradius/clients.conf” 文件读取nas的信息，可以修改成从数据库nas表读取nas信息

1
sudo sed -i 's/\#readclients/readclients/g' /etc/freeradius/sql.conf
3.6 打开在线人数查询支持

在文件/etc/freeradius/sql/mysql/dialup.conf中查找simul_count_query 将279-282行注释去掉

1
sudo vim /etc/freeradius/sql/mysql/dialup.conf
修改后的内容如下：

1
2
3
4
5
# Uncomment simul_count_query to enable simultaneous 
use checking simul_count_query = "SELECT COUNT(*) \
                             FROM ${acct_table1} \
                             WHERE username = '%{SQL-User-Name}' \
                             AND acctstoptime IS NULL"
 3.7 修改sites-enabled目录配置文件

编辑/etc/freeradius/sites-enabled/default，根据下面的说明注释或取消注释相应的行，行号仅供参考：

1
sudo vim /etc/freeradius/sites-enabled/default
找到authorize {}模块，注释掉files（152行），去掉sql前的#号（159行）
找到accounting {}模块，注释掉radutmp(378行),注释掉去掉sql前面的#号(388行)。
找到session {}模块，注释掉radutmp（432行），去掉sql前面的#号（436行）。
找到post-auth {}模块，去掉sql前的#号（457行），去掉sql前的#号（545行）。

1
sudo vim /etc/freeradius/sites-enabled/inner-tunnel
找到authorize {}模块，注释掉files（124行），去掉sql前的#号（131行）。
找到session {}模块，注释掉radutmp（251行），去掉sql前面的#号（255行）。
找到post-auth {}模块，去掉sql前的#号（277行）,去掉sql前的#号（301行）。

3.8 向数据库添加用户资料，权限等信息

3.8.1 连接 MySQL 数据库

1
mysql -uroot -p123456;
3.8.2 使用 radius 数据库

1
USE radius;
3.8.3 添加用户test，密码test，注意是在radchec表

1
INSERT INTO radcheck (username,attribute,op,VALUE) VALUES ('test','Cleartext-Password',':=','test');
3.8.4 将用户test加入VIP1用户组

1
INSERT INTO radusergroup (username,groupname) VALUES ('test','VIP1');
3.8.5 限制同时登陆人数，注意是在radgroupcheck表

1
INSERT INTO radgroupcheck (groupname,attribute,op,VALUE) VALUES ('VIP1','Simultaneous-Use',':=','3');
如果是针对某一个特定用户做限制，则可以加入radcheck表中。
3.8.6 添加NAS,把localhost换成服务器的ip地址。这里是192.168.11.40

1
INSERT INTO radius.nas VALUES ('1','192.168.11.40','marker123', 'other', NULL ,'marker123.com',NULL ,NULL ,'RADIUS Client');
3.8.7 添加一些基本的用户配置

1
2
3
4
5
INSERT INTO radgroupreply (groupname,attribute,op,VALUE) VALUES ('VIP1','Auth-Type',':=','Local');
INSERT INTO radgroupreply (groupname,attribute,op,VALUE) VALUES ('VIP1','Service-Type',':=','Framed-User');
INSERT INTO radgroupreply (groupname,attribute,op,VALUE) VALUES ('VIP1','Framed-Protocol',':=','PPP');
INSERT INTO radgroupreply (groupname,attribute,op,VALUE) VALUES ('VIP1','Framed-MTU',':=','1500');
INSERT INTO radgroupreply (groupname,attribute,op,VALUE) VALUES ('VIP1','Framed-Compression',':=','Van-Jacobson-TCP-IP');
3.9 测试

以debug模式启动freeradius

1
sudo freeradius -X
从Log 可以看到连接mysql是否成功，有可能会出现radius用户对raidius数据库的权限不够，我是通过phpmyadmin对radius用户权限进行了修改，追加了所有SQL操作权限。
新开一个窗口执行，看到 “Access-Accept packet” 表示成功了，”Access-Reject” 表示失败了。

1
radtest test test 192.168.11.40 0 marker123.com
4 安装及配置freeradius客户端

4.1 安装radiusclient

1
sudo apt-get install radiusclient1
4.2 设置和服务器的连接

其中192.168.11.40为nas表中插入的nasname，marker123.com为nas表中的nasname对应记录的secret:

1
2
3
sudo cat >>/etc/radiusclient/servers<<EOF
192.168.11.40   marker123.com
EOF
同时修改/etc/radiusclient/radiusclient.conf中的ip设置。要把192.168.11.40换成服务器的IP地址。

1
sudo sed -i 's/localhost/192.168.11.40/g' /etc/radiusclient/radiusclient.conf
4.3 设置字典

这一步很重要！否则windows客户端无法连接服务器。

1
2
3
4
5
6
7
8
9
wget -c http://small-script.googlecode.com/files/dictionary.microsoft
sudo mv ./dictionary.microsoft /etc/radiusclient/
sudo cat >>/etc/radiusclient/dictionary<<EOF
#INCLUDE /etc/radiusclient/dictionary.sip(这个文件不存，可以移除）
#INCLUDE /etc/radiusclient/dictionary.ascend(这个加入后，会和用户流量限流有冲突）
INCLUDE /etc/radiusclient/dictionary.merit
INCLUDE /etc/radiusclient/dictionary.compat
INCLUDE /etc/radiusclient/dictionary.microsoft
EOF
4.4 pptp启用freeradius插件

1
2
3
4
5
6
7
8
9
sudo sed -i 's/logwtmp/\#logwtmp/g' /etc/pptpd.conf
sudo sed -i 's/radius_deadtime/\#radius_deadtime/g' /etc/radiusclient/radiusclient.conf
(这句其实没作用)
sudo sed -i 's/bindaddr/\#bindaddr/g' /etc/radiusclient/radiusclient.conf
(这句也没用)
sudo cat >>/etc/ppp/pptpd-options <<EOF
plugin /usr/lib/pppd/2.4.5/radius.so
radius-config-file /etc/radiusclient/radiusclient.conf
EOF
5 启用VPN流量控制功能

5.1 启用 Rlm sqlcounter 模块

在/etc/freeradius/radiusd.conf文件中，查找”counter.conf”(695行)，去掉#号

1
sudo vim /etc/freeradius/radiusd.conf
修改后内容如下:

1
$INCLUDE sql/mysql/counter.conf
5.2 添加 Traffic Counter流量计数器

1
sudo vim /etc/freeradius/sql/mysql/counter.conf
在文件的最后添加如下内容

1
2
3
4
5
6
7
8
9
qlcounter monthlytrafficcounter {
    counter-name = Monthly-Traffic
    check-name = Max-Monthly-Traffic
    reply-name = Session-Octets-Limit
    sqlmod-inst = sql
    key = User-Name
    reset = monthly
    query = "SELECT IFNULL(SUM(acctinputoctets + acctoutputoctets),0) as counter FROM radacct WHERE UserName='%{%k}' AND UNIX_TIMESTAMP(AcctStartTime) > '%b'"
}
Max-Monthly-Traffic: 每个月最大流量
Session-Octets-Limit: 用户登录时，freeradius服务返回客户端，目前还剩多少流量。
上面SQL代码意思是按月进行统计，从数据库的radacct表中，根据用户名(%k)将所有入站和出站流量累加。如果本月没有流量，就为0。时间也是可以自定义的（months、weeks、days、hours），也可以指定具体值，如三天重置一次 “reset = 3 d”。

5.3 启用traffic counter流量计数器

编辑/etc/freeradius/sites-enabled/default

1
sudo vim /etc/freeradius/sites-enabled/default
在authorize区块的末尾（205行）添加

1
monthlytrafficcounter
5.4 添加字典文件

编辑字典文件/etc/freeradius/dictionary

1
sudo vim /etc/freeradius/dictionary
在文件末尾中添加下面两行

1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
ATTRIBUTE Max-Monthly-Traffic 3003 integer#
 
# Experimental, implementation specific attributes
#
# Limit session traffic
ATTRIBUTE Session-Octets-Limit 227 integer
# What to assume as limit - 0 in+out, 1 in, 2 out, 3 max(in,out)
ATTRIBUTE Octets-Direction 228 integer
 
# Octets-Direction
VALUE Octets-Direction Sum 0
VALUE Octets-Direction Input 1
VALUE Octets-Direction Output 2
VALUE Octets-Direction MaxOveral 3
VALUE Octets-Direction MaxSession 4
在/etc/radiusclient/dictionary中追加

1
2
3
4
5
6
7
8
9
10
11
12
13
14
#
# Experimental, implementation specific attributes
#
# Limit session traffic
ATTRIBUTE Session-Octets-Limit 227 integer
# What to assume as limit - 0 in+out, 1 in, 2 out, 3 max(in,out)
ATTRIBUTE Octets-Direction 228 integer
 
# Octets-Direction
VALUE Octets-Direction Sum 0
VALUE Octets-Direction Input 1
VALUE Octets-Direction Output 2
VALUE Octets-Direction MaxOveral 3
VALUE Octets-Direction MaxSession 4
5.5 在数据库插入流量限制值

注意事项：
1）这里插入到radgroupcheck表，是限制某个用户组的流量。也可以插入到radcheck表，以限制某个用户的流量。
2）流量值以 byte 为单位，1G = 1073741824 bytes
3）VIP1是用户组，123456是数据库root密码
每月最大流量（1G）

1
INSERT INTO radgroupcheck (groupname,attribute,op,VALUE) VALUES ('VIP1','Max-Monthly-Traffic',':=','1073741824');
流量统计时间的间隔（60秒）
参考资料：
每60秒更新一次流量至数据表

1
INSERT INTO radgroupreply (groupname,attribute,op,VALUE) VALUES ('VIP1','Acct-Interim-Interval',':=','60');
流量计算规则：0 in+out, 1 in, 2 out, 3 max(in,out)

1
INSERT INTO radgroupreply (groupname,attribute,op,VALUE) VALUES ('VIP1',' Octets-Direction ',':=','0')

