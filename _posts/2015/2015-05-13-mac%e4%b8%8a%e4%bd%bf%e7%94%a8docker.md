---
layout: post
title: Mac上使用Docker
date: 2015-05-13 11:09
author: admin
comments: true
categories: []
---

要在Mac上安裝Docker需要做特殊的處理，下面將一步步說明。

預先安裝 homebrew
Homebrew是Mac上相當傑出的套件管理程式，我們將用它來安裝docker，
安裝Homebrew相當容易，只要進到官網複製第一行 (E.g. ruby -e "$(curl -fsSL https:...)
貼到terminal然後enter即可。

> ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
安裝 docker
利用Homebrew安裝docker，在terminal輸入：

> brew update
> brew install docker
安裝完後，此時若去執行docker會出現錯誤

> docker run hello-world
> FATA[0000] Post http:///var/run/docker.sock/v1.18/containers/create: dial unix /var/run/docker.sock: no such file or directory. Are you trying to connect to a TLS-enabled daemon without TLS? 
  FAIL
原因出在docker需要linux kernel提供的feature，而OS X並沒提供，因此要進行接下來的步驟

安裝 Boot2Docker
Boot2Docker幫助我們在OS X建立Linux的虛擬環境，讓我們可以跑在Linux上，安裝一樣用brew

> brew install boot2docker
Boot2Docker需要安裝virtual box環境

> brew tap caskroom/cask
> brew cask install virtualbox
啟動 Boot2Docker
還記得剛提到的虛擬機器，用boot2docker幫我們初始化

> boot2docker init
啟動虛擬機器

> boot2docker up
等機器啟動好後，使用下列指令設定環境變數

> eval "$(boot2docker shellinit)"
看到下列訊息不要緊張，表示已經完成了！

Writing /Users/chinhui/.boot2docker/certs/boot2docker-vm/ca.pem
Writing /Users/chinhui/.boot2docker/certs/boot2docker-vm/cert.pem
Writing /Users/chinhui/.boot2docker/certs/boot2docker-vm/key.pem
可以將此指令寫入shrc中，自動設定環境變數，如

> cat ~/.zshrc

  ...
  # Use boot2docker for docker in MacOS
  eval "$(boot2docker shellinit)"        //加入這行  
  ...
測試 docker
可使用下面指令測試docker是否可以正確執行

> docker run hello-world
  Hello from Docker.
  This message shows that your installation appears to be working correctly.
  ...
Well done 成功安裝, 下一篇將介紹在boot2docker下對docker的操作
