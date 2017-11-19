---
layout: post
title: jquery get,eq取val
date: 2014-05-15 08:46
author: admin
comments: true
categories: []
---
function dothing() {
        var tds = $('#'+selected+' td');
        var submitvals = new Array();
        tds.each(function(i) {
            var val = $(this).children('input')[0].val();
            submitvals.push(val);
        });
    }
Theres more to the function, but this is all that is relevant. For some reason, when I run this code, I get "HTMLInputElement has no method 'val'." I thought that input elements were supposed to have a val() method in jQuery that got the value so this makes no sense. Am I missing something, or doing it wrong?

或者用.get(0).val()不行，
使用 .eq(0).val()可行。
val() is a jQuery method. .value is the DOM Element's property. Use [0].value or .eq(0).val()....

.val() is a jQuery function, not a javascript function. Therefore, change:

var val = $(this).children('input')[0].val()
To:

var val = $(this).children('input:eq(0)').val()
