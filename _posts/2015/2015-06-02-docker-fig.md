---
layout: post
title: docker fig
date: 2015-06-02 22:15
author: admin
comments: true
categories: []
---
docker run -d -P -v /Users/Chris/web:/usr/local/nginx/html --name web nginx

docker port web 80

fig


nginx:
  image: nginx
  links:
    - php

php:
  image: php
  ports:
    - "8000:8000"
  links:
    - mysql
  volumes:
    - /Users/apple/web:/code

mysql:
  image: mysql:5.6
  environment:
    MYSQL_DATABASE: root
    MYSQL_ROOT_PASSWORD: root
  volumes:
    - /usr/local/mysql/data:/db
~                                                                                    

