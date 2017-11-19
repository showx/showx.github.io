---
layout: post
title: VirtualBox使用心得
date: 2013-08-19 20:46
author: admin
comments: true
categories: []
---
常用命令：
以下所有命令在输入vboxmanager后都能看到，这里只是列几个用的多的。

显示所有虚拟机：vboxmanage list vms

显示所有正在运行的虚拟机：vboxmanage list runningvms

显示虚拟机详细信息：vboxmanage showvminfo winxp

修改虚拟机名称：vboxmanage modifyvm winxp --name winxp_clone

修改虚拟机所占用的cpu内核数：vboxmanage modifyvm winxp --cpus 2

启动虚拟机：vboxmanage startvm winxp --type headless

关闭虚拟机：vboxmanage controlvm winxp poweroff

注册虚拟机（绝对路径）：vboxmanage registervm       <filename>

修改端口映射规则：vboxmanage modifyvm winxp --natpf1 rule_ssh, tcp, , 8888, 10.0.2.15, 8888

将网络连接修改成桥接：vboxmanage modifyvm winxp --nic1 bridged --bridgeadapter1 eth0

设置虚拟机vrde的ip地址：vboxmanage modifyvm winxp --vrdeport 3388

 

心得一：虚拟机无法启动

     由于服务器需要断电，有同事就把服务器关掉了，但是服务器上的虚拟机并没有正常关闭，结果再开机的时候就显示下面的错误：

[root@localhost ~]# vboxmanage startvm winxp 
Waiting for VM "winxp" to power on... 
VBoxManage: error: The virtual machine 'winxp' has terminated unexpectedly during startup with exit code 0 
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component Machine, interface IMachine, callee  
 

    用help命令查了一下，启动的时候还有3个参数可以选用。

VBoxManage startvm          <uuid>|<name>... 
                            [--type gui|sdl|headless] 
 

    突然想起来，刚才用VNC连接就没成功，可能和界面有关系。干脆用无界面的方式启动，问题解决。

[root@localhost ~]# vboxmanage startvm winxp --type headless 
Waiting for VM "winxp" to power on... 
VM "winxp" has been successfully started. 
 

 

心得二：虚拟机无法注册

 vboxmanage registervm [path]无法注册，出现如下报错：

[root@localhost VirtualBox VMs]# vboxmanage registervm CentOS5.6_64\ Clone/ 
VBoxManage: error: Runtime error opening '/root/.VirtualBox/centos5.6_64 Clone' for reading: -102 (File not found.). 
VBoxManage: error: /home/vbox/vbox-4.1.2/src/VBox/Main/src-server/MachineImpl.cpp[436] (nsresult Machine::init(VirtualBox*, const com::Utf8Str&, const com::Guid*)) 
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component VirtualBox, interface IVirtualBox, callee nsISupports 
Context: "OpenMachine(Bstr(a->argv[0]).raw(), machine.asOutParam())" at line 90 of file VBoxManageMisc.cpp 
    在官方找到了答案，这里path必须是绝对路径！这个地方做的确认太不人性化了，具体见https://www.virtualbox.org/ticket/8468。

 

    如果是在备份虚拟机时直接注册，可以使用：
vboxmanage clonevm winxp --name winxp_loadrunner --register 
 
 

心得三：利用VRDE通过实体机远程连接虚拟机桌面

服务器上没有安装vnc，把clone的镜像文件放到虚拟机中重启以后突然发现无法连接上，因为和原来的虚拟机是使用同样的ip。如果用的nat方式，还比较方便，开启了dhcp肯定是可以的，利用port forward把另外一个主机的远程连接端口映射出来也没问题。但是如果是用bridge，那没修改ip怎么连接上呢？！

这里就需要用到VRDE，它是在实体机上开的一个远程桌面端口。这个和操作系统的远程桌面不一样，它是通过实体机来看虚拟机桌面，相当于直接在实体机上操作虚拟机，所以你能看到操作系统的开机画面。理论上，这样也可以在不用vnc连接实体机的情况下，安装虚拟机操作系统。我现在都是用装好的系统文件clone的，这个还没试过，有试过的兄弟告诉我一下。

通过vboxmanage modifyvm winxp --vrdeport 3388命令就可以修改虚拟机的端口，连接的时候是使用RDP协议，在windows下直接用mstsc，linux下用rdesktop就可以。这个和你虚拟机安装的操作系统无关。
