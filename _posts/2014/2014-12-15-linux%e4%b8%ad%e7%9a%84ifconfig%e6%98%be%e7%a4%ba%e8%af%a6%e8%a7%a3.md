---
layout: post
title: linux中的ifconfig显示详解
date: 2014-12-15 17:35
author: admin
comments: true
categories: []
---
[root@XXX ~]# ifconfig

eth0      Link encap:Ethernet  HWaddr 00:0C:29:C8:87:DB

inet addr:192.168.5.103  Bcast:192.168.5.255  Mask:255.255.255.0

inet6 addr: fe80::20c:29ff:fec8:87db/64 Scope:Link

UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1

RX packets:215632 errors:0 dropped:0 overruns:0 frame:0

TX packets:87 errors:0 dropped:0 overruns:0 carrier:0

collisions:0 txqueuelen:1000

RX bytes:15503343 (14.7 MiB)  TX bytes:10634 (10.3 KiB)

Interrupt:177 Base address:0x1400

HWaddr: 网卡MAC地址,网卡出厂时就固化在网卡里的。

inet addr： IP地址

Bcast: 广播地址。指定用于发送广播消息的 IP 地址。使用本地 IP 地址和子网掩码创建缺省广播地址。例如目的地址192.168.5.255表示广播至192.168.5.0网络上的所有主机。

Mask: 子网掩码。子网掩码指示哪部分 IP 地址识别网络，哪部分识别主机。internet被各种路由器和网关设备分隔成很多网段，为了标识不同的网段，需要把32位的IP地址划分成网络号和主机号两部分，网络号相同的各主机位于同一网段，相互间可以直接通信，网络号不同的主机之间通信则需要通过路由器转发。网络号和主机号的划分需要用一个额外的子网掩码（subnet mask）来表示，而不能由IP地址本身的数值决定，也就是说，网络号和主机号的划分与这个IP地址是A类、B类还是C类无关。这样，多个子网就可以汇总mmarize）成一个Internet上的网络，例如，有8个站点都申请了C类网络，本来网络号是24位的，但是这8个站点通过同一个ISP（Internet service provider）连到Internet上，它们网络号的高21位是相同的，只有低三位不同，这8个站点就可以汇总，在Internet上只需要一个路由表项，数据包通过Internet上的路由器到达ISP，然后在ISP这边再通过次级的路由器选路到某个站点。IP地址与子网掩码做与运算可以得到网络号，主机号从全0到全1就是子网的地址范围。IP地址和子网掩码还有一种更简洁的表示方法，例如 140.252.20.68/24，表示IP地址为140.252.20.68，子网掩码的高24位是1，也就是255.255.255.0。

实例1

IP地址    140.252.20.68    8C FC 14 44

子网掩码    255.255.255.0    FF FF FF 00

网络号    140.252.20.0    8C FC 14 00

子网地址范围    140.252.20.0~140.252.20.255

实例2

IP地址    140.252.20.68    8C FC 14 44

子网掩码    255.255.255.240    FF FF FF F0

网络号    140.252.20.64    8C FC 14 40

子网地址范围    140.252.20.64~140.252.20.79

inet6 addr: IPv6地址

UP（代表网卡开启状态）RUNNING（代表网卡的网线被接上）MULTICAST（支持组播）

MTU：最大传输单元。

以太网帧中的数据长度规定最小46字节，最大1500字节，ARP和RARP数据包的长度不够46字节，要在后面补填充位。最大值1500称为以太网的最大传输单元（MTU），不同的网络类型有不同的MTU，如果一个数据包从以太网路由到拨号链路上，数据包长度大于拨号链路的MTU了，则需要对数据包进行分片（fragmentation）。注意，MTU这个概念指数据帧中有效载荷的最大长度，不包括帧首部的长度。

RX和TX：

RX表示接收数据包的情况

TX表示发送数据包的情况

如果你的网卡已经完成配置却还是无法与其它设备通信，那么从RX 和TX 的显示数据上可以简单地分析一下故障原因。在这种情况下，如果你看到接收和传送的包的计数(packets)增加，那有可能是系统的IP 地址出现了混乱；如果你看到大量的错误(errors)和冲突(Collisions)，那么这很有可能是网络的传输介质出了问题，例如网线不通或hub损坏。

collisions： 网络讯号碰撞的情况说明

txqueuelen： 传输缓区长度大小
