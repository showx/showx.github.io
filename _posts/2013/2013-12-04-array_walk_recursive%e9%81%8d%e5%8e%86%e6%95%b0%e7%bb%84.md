---
layout: post
title: array_walk_recursive遍历数组
date: 2013-12-04 09:55
author: admin
comments: true
categories: []
---
<?php
class A_Class {

function array_walk_recursive(&$input, $funcname, $userdata = '') {
  if(!function_exists('array_walk_recursive')) {
    if(!is_callable($funcname))
      return false;

    if(!is_array($input))
      return false;

    foreach($input as $key=>$value) {
      if(is_array($input[$key])) {
        if(isset($this)) {
          eval('$this->' . __FUNCTION__ . '($input[$key], $funcname, $userdata);');
        } else {
          if(@get_class($this))
            eval(get_class() . '::' . __FUNCTION__ . '($input[$key], $funcname, $userdata);');
          else
            eval(__FUNCTION__ . '($input[$key], $funcname, $userdata);');
        }
      } else {
        $saved_value = $value;

        if(is_array($funcname)) {
          $f = '';
          for($a=0; $a<count($funcname); $a++)
            if(is_object($funcname[$a])) {
              $f .= get_class($funcname[$a]);
            } else {
              if($a > 0)
                $f .= '::';
              $f .= $funcname[$a];
            }
          $f .= '($value, $key' . (!empty($userdata) ? ', $userdata' : '') . ');';
          eval($f);
        } else {
          if(!empty($userdata))
            $funcname($value, $key, $userdata);
          else
            $funcname($value, $key);
        }

        if($value != $saved_value)
          $input[$key] = $value;
      }
    }
    return true;
  } else {
    array_walk_recursive($input, $funcname, $userdata);
  }
}

function kv_addslashes(&$v, $k) {
  $v = addslashes($v);
}
}
?>

Usage:
<?php
$arr = array(
  'a' => '"Hello World"',
  'b' => "'Hello World'",
  'c' => "Hello 'Worl\"d",
  'd' => array(
    'A' => 'H"e"l"l"o" "W"o"r"l"d'
    )
  );

$class = new A_Class();
$class->array_walk_recursive($arr, array(&$class, 'kv_addslashes'));
print_r($arr);
?>
