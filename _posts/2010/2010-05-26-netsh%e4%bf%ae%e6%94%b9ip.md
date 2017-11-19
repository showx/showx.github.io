---
layout: post
title: NETSH修改IP
date: 2010-05-26 08:56
author: admin
comments: true
categories: []
---
C:\Documents and Settings\aa&gt;netsh
netsh&gt;interface
netsh interface&gt;ip
netsh interface ip&gt;add address "本地连接" 192.168.10.5 255.255.255.0 192.168.10.1 1确定。
netsh interface ip&gt;
C:\Documents and Settings\aa&gt;netsh
netsh&gt;interface
netsh interface&gt;ip
netsh interface ip&gt;?
下列指令有效:
命令从 netsh 上下文继承:
.. ?? ?? ?? - 移到上一层上下文级。
abort ?? ?? - 丢弃在脱机模式下所做的更改。
add ?? ?? ?? - 在项目列表上添加一个配置项目。
alias ?? ?? - 添加一个别名
bridge ?? ?? - 更改到 `netsh bridge' 上下文。
bye ?? ?? ?? - 退出程序。
commit ?? ?? - 提交在脱机模式中所做的更改。
delete ?? ?? - 在项目列表上删除一个配置项目。
diag ?? ?? ??? - 更改到 `netsh diag' 上下文。
exit ?? ?? ??? - 退出程序。
interface ?? - 更改到 `netsh interface' 上下文。
offline ?? ??? - 将当前模式设置成脱机。
online ?? ?? - 将当前模式设置成联机。
popd ?? ?? ??? - 从堆栈上打开一个上下文。
pushd ?? ?? - 将当前上下文放推入堆栈。
quit ?? ?? ??? - 退出程序。
ras ?? ?? ?? - 更改到 `netsh ras' 上下文。
routing ?? ??? - 更改到 `netsh routing' 上下文。
set ?? ?? ?? - 更新配置设置。
show ?? ?? ??? - 显示信息
unalias ?? ??? - 删除一个别名。
命令从 netsh interface 上下文继承:
add ?? ?? ?? - 向表中添加一个配置项目。
delete ?? ?? - 从表中删除一个配置项目。
ip ?? ?? ?? - 更改到 `netsh interface ip' 上下文。
reset ?? ?? - 复位信息。
set ?? ?? ?? - 设置配置信息。
show ?? ?? ??? - 显示信息。
此上下文中的命令:
? ?? ?? ?? ??? - 显示命令列表。
add ?? ?? ?? - 向表中添加一个配置项目。
delete ?? ?? - 从表中删除一个配置项目。
dump ?? ?? ??? - 显示一个配置脚本。
help ?? ?? ??? - 显示命令列表。
reset ?? ?? - 复位 TCP/IP 及相关的组件到干净的状态。
set ?? ?? ?? - 设置配置信息。
show ?? ?? ??? - 显示信息。
若需要命令的更多帮助信息，请键入命令，
后面跟 ?。
netsh interface ip&gt;set
下列指令有效:
命令从 netsh 上下文继承:
set file ?? - 复制控制台输出到文件。
set machine - 设置用来操作的当前计算机。
set mode ?? - 设置当前模式为联机或脱机。
此上下文中的命令:
set address - 设置指定的接口的 IP 地址或默认网关。
set dns ?? ??? - 设置 DNS 服务器模式和地址。
set wins ?? - 设置 WINS 服务器模式和地址。
netsh interface ip&gt;set dns "本地连接" static 61.18.34.69
确定。
netsh interface ip&gt;add
下列指令有效:
命令从 netsh 上下文继承:
add helper ??? - 安装一个助手 DLL。

此上下文中的命令:
add address - 添加一个 IP 地址到指定的接口。
add dns ?? ??? - 添加一个静态 DNS 服务器地址。
add wins ?? - 添加一个静态 WINS 服务器地址。
netsh interface ip&gt;add dns
用法: add dns [name=]&lt;string&gt; [addr=]&lt;IP address&gt; [[index=]&lt;integer&gt;]
参数:
?? 标记 ?? ?? ??? 值
?? name ?? ?? - 添加 DNS 服务器的接口的名称。
?? addr ?? ?? - 添加的 DNS 服务器的 IP 地址。
?? index ?? ??? - 为指定的 DNS 服务器地址指定索引(首选项)。

注释: 把一个新的 DNS 服务器 IP 地址添加到静态配置的列表中。
?? ?? 默认情况下，这个DNS 服务器被添加在列表的结尾。如果指定一个索引，DNS 服
务器将被置于列表中指定的位置，其他服务器将被移后留出空间。如果 DNS 服务器以前是
通过 DHCP 获取的，这个新的地址将取代旧的列表。
示例:
?? add dns "Local Area Connection" 10.0.0.1
?? add dns "Local Area Connection" 10.0.0.3 index=2
netsh interface ip&gt;add dns "本地连接" 61.18.39.63
确定。
netsh interface ip&gt;
注：设置第二个DNS的时候就不能用set命令了，否则会盖掉第一个DNS配置，应该使用add dns子命令！
NetSh 命令
　　list 列出所有可用的 WINS 命令。
　　dump 将 WINS 服务器配置转储到命令输出。
　　add name 在服务器上注册名称。详细信息，请输入 add name /?
　　add partner 向服务器添加复制伙伴。详细信息，请输入 add partner /?
　　add pngserver 添加当前服务器的 Persona Non Grata 服务器列表。详细信息，请输入 add pngserver /?
　　check database 检查数据库的一致性。详细信息，请输入 check database /?
　　check name 检查一组 WINS 服务器的名称记录列表。详细信息，请输入 check name /?
　　check version 检查版本号的一致性。详细信息，请输入 check version /?
　　delete name 从服务器数据库中删除已注册的名称。详细信息，请输入 delete name /?
　　delete partner 从复制伙伴列表中删除复制伙伴。详细信息，请输入 delete partner /?
　　delete records 从服务器删除或逻辑删除所有记录或一组记录。详细信息，请输入 delete records /?
　　delete owners 删除所有者列表及其记录。详细信息，请输入 delete owners /?
　　delete pngserver 从列表中删除所有的或选定的 Persona Non Grata 服务器。详细信息，请输入 delete pngserver /?
　　init backup 备份 WINS 数据库。详细信息，请输入 init backup /?
　　init import 从 Lmhosts 文件导入数据。详细信息，请输入 init import /?
　　init pull 启动“拉”触发器，并发送给另一台 WINS 服务器。详细信息，请输入 init pull /?
　　init pullrange 开始另一台 WINS 服务器的一组记录，并读取该记录。详细信息，请输入 init pullrange /?
　　init push 启动“推”触发器，并发送给另一台 WINS 服务器。详细信息，请输入 init push /?
　　init replicate 用复制伙伴复制数据库。详细信息，请输入 init replicate /?
　　init restore 从文件还原数据库。详细信息，请输入 init restore /?
　　init scavenge 清除服务器的 WINS 数据库。详细信息，请输入 init scavenge /?
　　init search 搜索服务器的 WINS 数据库。详细信息，请输入 init search /?
　　reset statistics 重置服务器的统计信息。详细信息，请输入 reset statistics /?
　　set autopartnerconfig 设置服务器的自动复制伙伴配置信息。详细信息，请输入 set autopartnerconfig /?
　　set backuppath 设置服务器的备份参数。详细信息，请输入 set backuppath /?
　　set burstparam 设置服务器的突发处理参数。详细信息，请输入 set autopartnerconfig /?
　　set logparam 设置数据库和事件日志记录选项。详细信息，请输入 set logparam /?
　　set migrateflag 设置服务器的迁移标志。详细信息，请输入 set migrateflag /?
　　set namerecord 设置服务器的间隔和超时值。详细信息，请输入 set namerecord /?
　　set periodicdbchecking 设置服务器的定期数据库检查参数。详细信息，请输入 set periodicdbchecking /?
　　set pullpartnerconfig 设置指定的“拉”伙伴的配置参数。详细信息，请输入 set pullpartnerconfig /?
　　set pushpartnerconfig 设置指定的“推”伙伴的配置参数。详细信息，请输入 set pushpartnerconfig /?
　　set pullparam 设置服务器的默认“拉”参数。详细信息，请输入 set pullparam /?
　　set pushparam 设置服务器的默认“推”参数。详细信息，请输入 set pushparam /?
　　set replicateflag 设置服务器的复制标志。详细信息，请输入 set replicateflag /?
　　set startversion 设置数据库的开始版本 ID。详细信息，请输入 set startversion /?
　　show browser 显示所有活动域主浏览器的 [1Bh] 记录。详细信息，请输入 show browser /?
　　show database 显示指定服务器的数据库和记录。详细信息，请输入 show database /?
　　show info 显示配置信息。详细信息，请输入 show info /?
　　show name 显示服务器中特定记录的详细信息。详细信息，请输入 show name /?
　　show partner 显示服务器的“拉”或“推”（或“推拉”）伙伴。详细信息，请输入 show partner /?
　　show partnerproperties 显示默认伙伴配置。详细信息，请输入 show partnerproperties /?
　　show pullpartnerconfig 显示“拉”伙伴的配置信息。详细信息，请输入 show pullpartnerconfig /?
　　show pushpartnerconfig 显示“推”伙伴的配置信息。详细信息，请输入 show pushpartnerconfig /?
　　show reccount 显示指定服务器所拥有的记录数量。详细信息，请输入 show reccount /?
　　show recbyversion 显示指定服务器所拥有的记录。详细信息，请输入 show recbyversion /?
　　show server 显示当前选定的服务器。详细信息，请输入 show server /?
　　show statistics 显示 WINS 服务器的统计信息。详细信息，请输入 show statistics /?
　　show version 显示 WINS 服务器的当前版本计数器值。详细信息，请输入 show version /?
　　show versionmap 显示所有者 ID 到“最大版本数”的映射。详细信息，请输入 show versionmap /?
　　Interface 命令
　　interface set/show interface 启用、禁用、连接、断开连接以及显示请求拨号接口的配置。
　　interface set/show credentials 在请求拨号接口上配置或显示用户名、密码和域名。
下面提供一个修改IP,DNS,网关的批处理,在我电脑里翻出来的,具体来自哪里已无从考证,我加了注释,还望原作者见谅
@rem 使用方法:ipcmd 连接名称 IP地址 子网掩码 默认网关 DNS 备用DNS
@rem 例子:ipcmd "本地连接" 192.168.0.2 255.255.255.0 192.168.0.1 192.168.0.1
@Rem %1:Connection Name %2:IP Address %3:Sub Mask %4:Default Gateway %5:DNS %6:StandbyDNS
@echo off
@rem 如果连接名称为空则退出
if "%1"=="null" goto end
@rem 如果IP为空,则执行自动获取IP的命令
if "%2"=="null" goto autoIP
@rem 如果子网掩码空则退出
if "%3"=="null" goto end
@rem 如果没网关则执行没网关的命令
if "%4"=="null" goto notDefaultGateway
netsh interface ip set address %1 static %2 %3 %4 1
goto setDNS
:autoIP
netsh interface ip set address %1 source=dhcp
goto setDNS
:notDefaultGateway
netsh interface ip set address %1 static %2 %3
goto setDNS
:setDNS
if "%5"=="null" goto autoDNS
netsh interface ip set dns %1 static %5
goto standbyDNS
:autoDNS
netsh interface ip set dns %1 source=dhcp
goto end
:standbyDNS
if "%6"=="null" goto end
netsh interface ip add dns %1 %6
goto end
:end
