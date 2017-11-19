---
layout: post
title: Mac上使用Docker (2)
date: 2015-05-13 11:11
author: admin
comments: true
categories: []
---
<h3>Boot2Docker原理</h3>
之前提到Mac環境無法直接執行docker，所以在開了一台虛擬機器跑docker

Linux環境的docker如下 (來自官網的圖)

<img src="https://docs.docker.com/installation/images/linux_docker_host.png" alt="" />

DOCKER_HOST為提供安裝Docker的平台 (E.g. Linux, Mac)
Docker Daemon是即為Docker程式

可以看到Client是可以直接連到Docker Daemon，假設Container (E.g. Apache Server) 有開放Port 8000，則可以透過<code>localhost:8000</code> 連到。

Mac環境下的docker如下

<img src="https://docs.docker.com/installation/images/mac_docker_host.png" alt="" />

可以看到多了一橘色區域，就是boot2docker啟動的虛擬機器，Client要溝通則需透過虛擬機器的IP。假設IP為172.16.10.2，其中Port 5050對應到Container的Port 8000，則需要用 <code>172.16.10.2:5050</code> 去連。
<h3>nginx 範例</h3>
nginx是最近火紅的Web Server，接下來將透過安裝nginx了解Mac下的IP對應。

啟動nginx container

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; docker run -d -P --name web nginx
</pre>
</div>
</figure><code>-d</code>, <code>-P</code> 將會在後面介紹，<code>--name web</code>定這nginx命名為web，稍後會用到。這指令自動安裝nginx並且啟動。

現在我們想知道nginx的port跟虛擬機器port的對應

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; docker port web
  443/tcp -&gt; 0.0.0.0:32768
  80/tcp -&gt; 0.0.0.0:32769
</pre>
</div>
</figure>可以看到前面是nginx的port (80)，後面是docker host的port (32769)。

但由於Mac是需要透過虛擬機器，我們要知道虛擬機器的IP

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; boot2docker ip
  192.168.59.103
</pre>
</div>
</figure>在瀏覽器中打上 192.168.59.103:32769

<img src="http://user-image.logdown.io/user/12598/blog/11868/post/263794/52gi2fbRSMSDlxd1PYxI_Screenshot%202015-05-07%2007.17.33.png" alt="Screenshot 2015-05-07 07.17.33.png" />

就可成功連線

最後將nginx停止，刪除

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; docker stop web
&gt; docker rm web
</pre>
</div>
</figure>
<h3>更多 boot2docker</h3>
下次重開機後，要使用docker記得先啟動虛擬機器

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; boot2docker up
</pre>
</div>
</figure>在設定環境變數

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; eval "$(boot2docker shellinit)"
</pre>
</div>
</figure>如果不用了也可以自行關閉

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; boot2docker poweroff
</pre>
</div>
</figure>想查看現在狀態

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; boot2docker status
</pre>
</div>
</figure>如果發現boot2docker的IP連不上，有可能是你換過了網路，需要boot2docker重新取得IP，遇到這種情況，重新啟動即可

<figure class="figure-code code">
<div class="highlight">
<pre>&gt; boot2docker restart</pre>
</div>
</figure>
