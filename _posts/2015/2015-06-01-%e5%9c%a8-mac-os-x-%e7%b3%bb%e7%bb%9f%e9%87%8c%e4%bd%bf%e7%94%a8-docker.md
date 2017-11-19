---
layout: post
title: 在 Mac OS X 系统里使用 Docker
date: 2015-06-01 20:56
author: admin
comments: true
categories: []
---
目前的 Docker实现是建立在 Linux CGroup 等技术之上，因此无法在 MacOS X上原生使用(不过都折腾libcontainer了，不久应该会很快会有原生版本了吧)。需要建立一个 Linux虚拟机，然后在虚拟机内的 Linux上安装使用。为了简化整个安装使用，boot2docker.io提供了一个完全自包含的安装包，它会：

检测 Virtual Box虚拟机软件，如果没有则安装之，有则启动运行之。
创建名为  boot2dock-vm的 headless vm，这个 vm 非常精简，只提供了运行 docker所需要的基本环境，比自己从头安装一个 Linux省很多。
在 Mac OS X Host上安装 boot2docker 及 docker命令。
在 Mac OS X Host上安装 boot2docker app。这个 app 其实就打包了一个 Apple Script脚本，
它会打开一个系统 Terminal并启动虚拟机并做一些环境设置。在初次运行时会把 /usr/local/share/boot2docker/boot2docker.iso复制到 ~/.boot2docker/里，然后调用boot2docker init完成初始化。
因此，在习惯使用的Terminal软件如 iTerm2里，自己调用boot2docker up也是一样的。boot2docker启动之后，就可以在 Mac OS X的环境里使用 docker命令工作了，用法跟标准的 lxc-docker一样，它知道通过ssh将工作转发给虚拟机里的 docker实现。如果想要直接在虚拟机内工作，用 boot2docker ssh就要以获得一个运行在虚拟机内的shell了。

 

在 Mac OS X下，除了 boot2docker.io外，还有一个 skitematic 也可以提供 docker环境。Skitematic实际上是在 boot2docker的基础上又做了一层包装并提供 GUI方式管理docker以及访问 docker hub。Skitematic挺好用的，但是它其实是个web app，并且内部依赖http://fb.me/react-devtools，所以第一次使用需要翻墙否则界面不正常。更重要的时，它带的 boot2docker 注意了会用 dev做为 Virtual Box虚拟机的名称，但是放在 Mac OS X里的其它文件跟独立安装的 boot2docker是有冲突的。

 

在配置好 boot2docker之后又使用Skitematic的结果是先后出现了以下2个问题：

 

docker命令版本(1.1.8)变得比独立安装的 boot2docker-vm里的(1.17)要高，访问boot2docker-vm会报以下错误：
FATA[0000] Error response from daemon: client and server don't have same version (client : 1.18, server: 1.17)
由于 Skitematic GUI没有关闭VM的界面，需要File | Open Docker Command Line Console 然后 boot2docker down或得用 VirtualBox app的 VM管理功能关闭之，然后用 boot2docker.io的安装包安装取得老的 docker命令。
如果需要让两者都能用，可以把 1.17版本的 /usr/local/bin/docker备分到 /usr/local/bin/docker117，以后记得有 docker117为访问 boot2dcker-vm。或者只用 boot2docker ssh取得虚拟机上的shell工作。
来回折腾之后，boot2docker console会错误的尝试用 domain socket而不是 ssh连接 boot2dock-vm，从而报下面的错误：
FATA[0000] Gethttp:///var/run/docker.sock/v1.17/images/json: dial unix /var/run/docker.sock: no such file or directory. Are you trying to connect to a TLS-enabled daemon without TLS?
如果用 boot2docker app打开 console的话，会看到：
 
bash-3.2$/usr/local/bin/boot2docker up 

Waitingfor VM and Docker daemon to start...

...........ooo

Started.

Writing/Users/pinxue/.boot2docker/certs/boot2docker-vm/ca.pem

Writing/Users/pinxue/.boot2docker/certs/boot2docker-vm/cert.pem

Writing/Users/pinxue/.boot2docker/certs/boot2docker-vm/key.pem

 

Toconnect the Docker client to the Docker daemon, please set:

   export DOCKER_TLS_VERIFY=1

   export DOCKER_HOST=tcp://192.168.59.103:2376

   export DOCKER_CERT_PATH=/Users/pinxue/.boot2docker/certs/boot2docker-vm

 

bash-3.2$$(/usr/local/bin/boot2docker shellinit)

Writing/Users/pinxue/.boot2docker/certs/boot2docker-vm/ca.pem

Writing/Users/pinxue/.boot2docker/certs/boot2docker-vm/cert.pem

Writing/Users/pinxue/.boot2docker/certs/boot2docker-vm/key.pem

bash-3.2$docker version

Clientversion: 1.5.0

ClientAPI version: 1.17

Goversion (client): go1.4.1

Gitcommit (client): a8a31ef

OS/Arch(client): darwin/amd64

Serverversion: 1.5.0

ServerAPI version: 1.17

Goversion (server): go1.4.1

Gitcommit (server): a8a31ef

bash-3.2$ 

把提示的三个 DOCKER_环境变量设置一下就好了。
 

使用 boot2docker时，所有的 container都在boot2docker up命令启动的 Virtual Box VM里，container使用的端口通过 docker -P或者 -p映射到了 VM里的 LinuxHost上，但是在 Mac OS X里是没有的。从本机倒是可能用VM的ip访问到 container，从移动设备或其它机器上需要访问 container时，就需要在 Mac OS X上再做一次端口映射(portmapping)。有两个方法，在https://github.com/boot2docker/boot2docker/blob/master/doc/WORKAROUNDS.md里有介绍：

在 Mac OS X与 VM Linux之间临时建立 ssh tunnel
boot2docker ssh -vnNTL 8000:localhost:8000
用 Virtual Box的 NAT端口映射能力建立永久性的映射
虚拟机已关闭时：VBoxManage modifyvm "boot2docker-vm" --natpf1 "tcp-port8000,tcp,,8000,,8000";
虚拟机在运行时：VBoxManage controlvm "boot2docker-vm" natpf1 "tcp-port8000,tcp,,8000,,8000";
 

** 练练手还是不错的，不过真的挺容易把自己绕晕，开个海外的便宜 VPS 操练会更好，pull 时速度也会好很多。
