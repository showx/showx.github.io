---
layout: post
title: js 报错 Illegal break statement
date: 2015-05-20 09:27
author: admin
comments: true
categories: []
---
[javascript] view plaincopy
var a = {"a":1,"b":2,"c":3};  
var flag=0;  
for(var i in a){  
    $.ajax({  
        type:'GET',  
        url:'a.php',  
        success:function(){  
            if(i=="b"){  
                alert(a[i]);  
                break;  
            }  
        }  
    })  
}  

break 不能用在 function 里 
