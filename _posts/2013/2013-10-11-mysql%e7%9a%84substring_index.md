---
layout: post
title: mysql的SUBSTRING_INDEX  
date: 2013-10-11 11:09
author: admin
comments: true
categories: []
---
SUBSTRING_INDEX(str,delim,count) 

Returns the substring from string str before count occurrences of the delimiter delim. If count is positive, everything to the left of the final delimiter (counting from the left) is returned. If count is negative, everything to the right of the final delimiter (counting from the right) is returned. SUBSTRING_INDEX() performs a case-sensitive match when searching for delim. 

mysql> SELECT SUBSTRING_INDEX('www.mysql.com', '.', 2);
        -> 'www.mysql'
mysql> SELECT SUBSTRING_INDEX('www.mysql.com', '.', -2);
        -> 'mysql.com'

This function is multi-byte safe. 
