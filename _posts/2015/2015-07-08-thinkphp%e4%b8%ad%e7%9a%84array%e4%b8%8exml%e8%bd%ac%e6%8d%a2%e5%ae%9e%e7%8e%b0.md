---
layout: post
title: ThinkPHP中的Array与XML转换实现
date: 2015-07-08 10:06
author: admin
comments: true
categories: []
---
大家都知道，流年在TP中实现了一个数组到XML格式转换的xml_encode()函数；今天正巧想使用一下，发现其非常方便，方便的同时给添加了一个xml_decode()函数，用来把数据从编码后的xml字符串中还原成PHP数组。完整的代码如下：
<?php
function xml_encode($data, $charset = 'utf-8', $root = 'so') {
    $xml = '<?xml version="1.0" encoding="' . $charset .'"?>';
    $xml .= "<{$root}>";
    $xml .= array_to_xml($data);   
    $xml .= "</{$root}>";
    return $xml;
}

function xml_decode($xml, $root = 'so') {
    $search = '/<(' . $root . ')>(.*)<\/\s*?\\1\s*?>/s';
    $array = array();
    if(preg_match($search, $xml, $matches)){
        $array = xml_to_array($matches[2]);
    }
    return $array;
}

function array_to_xml($array) {
    if(is_object($array)){
        $array = get_object_vars($array);
    }
    $xml = '';
    foreach($array as $key => $value){
        $_tag = $key;
        $_id = null;
        if(is_numeric($key)){
            $_tag = 'item';
            $_id = ' id="' . $key . '"';
        }
        $xml .= "<{$_tag}{$_id}>";
        $xml .= (is_array($value) || is_object($value)) ? array_to_xml($value) : htmlentities($value);
        $xml .= "</{$_tag}>";
    }
    return $xml;
}

function xml_to_array($xml) {
    $search = '/<(\w+)\s*?(?:[^\/>]*)\s*(?:\/>|>(.*?)<\/\s*?\\1\s*?>)/s';
    $array = array ();
    if(preg_match_all($search, $xml, $matches)){
        foreach ($matches[1] as $i => $key) {
            $value = $matches[2][$i];
            if(preg_match_all($search, $value, $_matches)){
                $array[$key] = xml_to_array($value);
            }else{
                if('ITEM' == strtoupper($key)){
                    $array[] = html_entity_decode($value);
                }else{
                    $array[$key] = html_entity_decode($value);
                }
            }
        }
    }
    return $array;
}
附加一个简单的列子：
<?php
$array = array(
    0 => 'user',
    'name' => 'spring',
    'email' => 'mail@mail.com',
    'address' => array(
        'postcode' => 255000,
        'street' => 'some where for delay!'
    ),
    'friend' => array(
        0 => 'one',
        1 => 'two',
        2 => 'three',
        3 => 'four'
    )
);

$xml = xml_encode($array);

$array = xml_decode($xml);

print_r($array);
