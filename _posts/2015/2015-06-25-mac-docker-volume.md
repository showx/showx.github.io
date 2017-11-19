---
layout: post
title: mac docker volume
date: 2015-06-25 17:46
author: admin
comments: true
categories: []
---
[Mac] 安装osxfuse和sshfs, 下载地址: http://osxfuse.github.io

# 注：这里我之前用命令行安装osxfuse和sshfs，但运行后会有以下错误, 建议用官网的dmg包安装
$ the OSXFUSE file system is not available
[Mac] 主机上创建文件$HOME/.boot2docker/b2d-passwd

$ echo 'tcuser' > $HOME/.boot2docker/b2d-passwd
[Mac] 创建共享目录$HOME/docker/share $ mkdir -p $HOME/docker/share
[boot2docker] 进入boot2docker虚拟机执行, 进入命令./boot2docker ssh

# sudo mkdir /mnt/sda1/share
# sudo chown -R docker:docker /mnt/sda1/share
[Mac] 挂载

$ sshfs docker@localhost:/mnt/sda1/share $HOME/docker/share -oping_diskarb,volname=share -p 2022 -o reconnect -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -o password_stdin < ~/.boot2docker/b2d-passwd
[Mac] 卸载

$ umount -f $HOME/docker/share
