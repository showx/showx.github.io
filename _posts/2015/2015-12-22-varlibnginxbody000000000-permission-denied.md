---
layout: post
title: /var/lib/nginx/body/000000000 Permission denied
date: 2015-12-22 10:24
author: admin
comments: true
categories: []
---
排除权限问题之后，有可能是：
<pre><code>client_body_buffer_size     10M;
client_max_body_size        10M;
设置相应的body大小即可。</code></pre>
