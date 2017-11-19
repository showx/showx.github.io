---
layout: post
title: redis list操作
date: 2015-07-14 10:10
author: admin
comments: true
categories: []
---
redis 127.0.0.1:6379[1]&gt; lpush program java

(integer) 1

redis 127.0.0.1:6379[1]&gt; lpush program javascript

(integer) 2

redis 127.0.0.1:6379[1]&gt; lpush program javascript ruby

(integer) 4

redis 127.0.0.1:6379[1]&gt; llen program

(integer) 4

redis 127.0.0.1:6379[1]&gt; ltrim program 0 -1

OK

redis 127.0.0.1:6379[1]&gt; lrange program 0 -1

1) "ruby"

2) "javascript"

3) "javascript"

4) "java"

redis 127.0.0.1:6379[1]&gt; lpush program html

(integer) 5

redis 127.0.0.1:6379[1]&gt; lrange program 0 -1

1) "html"

2) "ruby"

3) "javascript"

4) "javascript"

5) "java"

redis 127.0.0.1:6379[1]&gt; rpush program redis

(integer) 6

redis 127.0.0.1:6379[1]&gt; lrange program 0 -1

1) "html"

2) "ruby"

3) "javascript"

4) "javascript"

5) "java"

6) "redis"

redis 127.0.0.1:6379[1]&gt; ltrim program 0 4

OK

redis 127.0.0.1:6379[1]&gt; llen program

(integer) 5

redis 127.0.0.1:6379[1]&gt; lrange program 0 -1

1) "html"

2) "ruby"

3) "javascript"

4) "javascript"

5) "java"

redis 127.0.0.1:6379[1]&gt; lrem program 1 javascript

(integer) 1

redis 127.0.0.1:6379[1]&gt; lrange 0 -1

(error) ERR wrong number of arguments for 'lrange' command

redis 127.0.0.1:6379[1]&gt; lrange program 0 -1

1) "html"

2) "ruby"

3) "javascript"

4) "java"

redis 127.0.0.1:6379[1]&gt; rpush program java

(integer) 5

redis 127.0.0.1:6379[1]&gt; lrange program 0 -1

1) "html"

2) "ruby"

3) "javascript"

4) "java"

5) "java"

redis 127.0.0.1:6379[1]&gt; lrem program 0 java

(integer) 2

redis 127.0.0.1:6379[1]&gt; llen program

(integer) 3

redis 127.0.0.1:6379[1]&gt; lpush program ruby

(integer) 4

redis 127.0.0.1:6379[1]&gt; lrange program 0 -1

1) "ruby"

2) "html"

3) "ruby"

4) "javascript"

redis 127.0.0.1:6379[1]&gt; lrem program -1 ruby

(integer) 1

redis 127.0.0.1:6379[1]&gt; lrange program 0 -1

1) "ruby"

2) "html"

3) "javascript"
