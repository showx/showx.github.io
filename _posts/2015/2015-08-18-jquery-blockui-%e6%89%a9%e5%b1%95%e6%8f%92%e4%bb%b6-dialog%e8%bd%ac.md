---
layout: post
title: jquery blockUI 扩展插件 Dialog[转]
date: 2015-08-18 14:42
author: admin
comments: true
categories: []
---
对jQuery blockUI插件进行了小的封装扩展，支持confirm、alert、dialog弹出窗口提示信息，支持按钮回调事件。可以自定义css样式、覆盖blockUI的样式等

首先要到jquery blockUI 官方网址：http://malsup.com/jquery/block/

下载jquery.blockUI JS lib：http://malsup.com/jquery/block/jquery.blockUI.js?v2.38

而且还需要jquery lib：http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js

 

jquery.blockUI.dialog.js

/***
 * jquery blockUI Dialog plugin 
 * v1.1 
 * @createDate -- 2012-1-4
 * @author hoojo
 * @email hoojo_@126.com
 * @requires jQuery v1.2.3 or later, jquery.blockUI-2.3.8
 * Copyright (c) 2012 M. hoo
 * Dual licensed under the MIT and GPL licenses:
 * http://hoojo.cnblogs.com
 * http://blog.csdn.net/IBM_hoojo
 **/
 
;(function ($) {
    
    var _dialog = $.blockUI.dialog = {};
    
    // dialog 默认配置
    var defaultOptions = {
        css: { 
            padding: '8px', 
            opacity: .7, 
            color: '#ffffff', 
            border: 'none', 
            'border-radius': '10px', 
            backgroundColor: '#000000' 
        },
        
        // 默认回调函数 取消或隐藏 dialog
        emptyHandler: function () {
            $.unblockUI();
        },
        
        // 用户回调函数
        callbackHandler: function (fn) {
            return function () {
                fn();
                defaultOptions.emptyHandler();
            };
        },
        
        // confirm 提示 html元素
        confirmElement: function (settings) {
            settings = settings || {};
            var message = settings.message || "default confirm message!";
            var okText = settings.okText || "ok";
            var cancelText = settings.cancelText || "cancel";
            
            if (typeof settings.onOk !== "function") {
                settings.onOk = this.emptyHandler;
            }
            if (typeof settings.onCancel !== "function") {
                settings.onCancel = this.emptyHandler;
            }
            var okCallback = this.callbackHandler(settings.onOk) || this.emptyHandler;
            var cancelCallback = this.callbackHandler(settings.onCancel) || this.emptyHandler;
            
            var html = [
                '<div class="dialog confirm">',
                '<p>',
                message,
                '</p>',
                '<input type="button" value="',
                okText,
                '" class="btn ok-btn"/>',
                '<input type="button" value="',
                cancelText,
                '" class="btn cancel-btn"/>'
            ].join("");
            
            var $el = $(html);
            $el.find(":button").get(0).onclick =  okCallback;
            $el.find(":button:last").get(0).onclick = cancelCallback;
            return $el;
        },
        
        // alert 提示html元素
        alertElement: function (settings) {
            settings = settings || {};
            var message = settings.message || "default alert message!";
            var okText = settings.okText || "ok";
            
            if (typeof settings.onOk !== "function") {
                settings.onOk = this.emptyHandler;
            }
            
            var okCallback = this.callbackHandler(settings.onOk) || this.emptyHandler;
            
            var html = [
                '<div class="dialog alert">',
                '<p>',
                message,
                '</p>',
                '<input type="button" value="',
                okText,
                '" class="btn ok-btn"/>'
            ].join("");
            
            var $el = $(html);
            
            $el.find(":button:first").get(0).onclick =  okCallback;
            return $el;
        }
    };
    
    var _options = defaultOptions;
    
    // 对外公开的dialog提示html元素，提供配置、覆盖
    $.blockUI.dialog.confirmElement = function () {
        return _options.confirmElement(arguments[0]);
    };
    
    $.blockUI.dialog.alertElement = function () {
        return _options.alertElement(arguments[0]);
    };
    
    // 提供jquery 插件方法
    $.extend({
        confirm: function (opts) {
            var i = (typeof opts === "object") ? 1 : 0;
            var defaults = {
                message: arguments[i++] || "default confirm message!",
                onOk: arguments[i++] || _options.emptyHandler(),
                onCancel: arguments[i++] || _options.emptyHandler(),
                okText: arguments[i++] || "ok",
                cancelText: arguments[i] || "cancel"
            };
            opts = opts || {};
            opts.css = $.extend({}, _options.css, opts.css);
            
            // 覆盖默认配置
            var settings = $.extend({}, _options, defaults, opts);
            settings = $.extend(settings, { message: _dialog.confirmElement(settings) });
            settings = $.extend({}, $.blockUI.defaults, settings);
            $.blockUI(settings);
        },
        alert: function (opts) {
            var i = (typeof opts === "object") ? 1 : 0;
            
            var defaults = {
                message: arguments[i++] || "default alert message!",
                onOk: arguments[i++] || _options.emptyHandler(),
                okText: arguments[i] || "ok"
            };
            
            opts = opts || {};
            opts.css = $.extend({}, _options.css, opts.css);
            
            var settings = $.extend({}, _options, defaults, opts);
            settings = $.extend(settings, { message: _dialog.alertElement(settings) });
            settings = $.extend({}, $.blockUI.defaults, settings);
            $.blockUI(settings);
        },
        
        // dialog提示
        dialog: function (opts) {
            var settings = $.extend({}, $.blockUI.defaults, _options, opts || {});
            $.blockUI(settings);
        },
        // 移除dialog提示框
        removeDialog: function () {
            _options.emptyHandler();
        }
    });
})(jQuery);
 

应用样式文件block.dialog.css

dialog是全局样式，btn是所有按钮的样式、ok-btn是ok按钮样式、cancel-btn是取消按钮样式

.dialog {
    font-size: 12px;
}
 
.dialog .btn {
    border: 1px solid white;
    border-radius: 5px;
    min-width: 25%;
    width: auto;
}
 
.dialog .ok-btn {
    background-color: #ccc;
}
 
.dialog .cancel-btn { 
    /*background-color: #aeface;*/
    margin-left: 10%;
}
 

examples.html

<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html>
  <head>
    <title>blockUI Dialog Examples</title>
    
    <meta http-equiv="author" content="hoojo">
    <meta http-equiv="content-type" content="text/html; charset=UTF-8">
    
    <link rel="stylesheet" href="blockUI/block.dialog.css" />
    <script type="text/javascript" charset="UTF-8" src="mobile/jquery-1.6.4.js"></script>
    <script type="text/javascript" charset="UTF-8" src="blockUI/jquery.blockUI-2.3.8.js"></script>
    <script type="text/javascript" charset="UTF-8" src="blockUI/jquery.blockUI.dialog-1.1.js"></script>
    
    <script type="text/javascript">
        ;(function ($) {
            $(function () {
            
                // dialog 提示框
                $("#dialog").click(function () {
                    //$.dialog();
                    //$.dialog({message: $("#callback")});
                    $.dialog({
                        //theme: true, // 设置主题需要jquery UI http://www.malsup.com/jquery/block/theme.html                
                        title: "dialog",
                        message: $("#callback"),
                        fadeIn: 1000,
                        fadeOut: 1200
                    });
                    setTimeout($.removeDialog, 2000);
                });
                
                // 带确定、取消按钮提示框，支持事件回调，及message等属性配置
                $("#confirm").click(function () {
                    $.confirm({
                        message: "你确定要删除吗？",
                        okText: "确定",
                        cancelText: "取消",
                        onOk: function () {
                            alert("你点击了确定按钮");
                        },
                        onCancel: function () {
                            alert("你点击了取消按钮");
                        }
                    });
                });    
                
                // 警告提示框 对象模式配置css、message、按钮文本提示
                $("#alert").click(function () {
                    $.alert({
                        message: "你确定要删除吗？",
                        okText: "确定",
                        css: {
                            "background-color": "white",
                            "color": "red"
                        },
                        onOk: function () {
                            alert("你点击了确定按钮");
                        }
                    });
                });
                
                // 非对象配置属性，多个参数配置
                /**
                  $.confirm 方法参数config配置介绍：
                  当第一个参数是一个对象：
                  对象中可以出现以下属性配置，并且可以出现$.blockUI的配置
                      {
                        message: arguments[i++] || "default confirm message!",
                        onOk: arguments[i++] || _options.emptyHandler(),
                        onCancel: arguments[i++] || _options.emptyHandler(),
                        okText: arguments[i++] || "ok",
                        cancelText: arguments[i] || "cancel"
                    }
                    message 是提示框的提示信息
                    onOk是确定按钮的click回调函数
                    onCancel 是取消按钮的click事件回调方法
                    okText是ok按钮的文字 默认是ok
                    cancelText是cancel按钮的文本内容
                  
                  如果第一个参数不是一个对象，那么
                      参数1表示 message 及提示文字
                    参数2表示ok按钮的click事件回调函数
                    参数3表示cancel按钮的click事件回调函数
                    参数4表示ok按钮的文本
                    参数5表示cancel按钮的文本内容
                    
                  注意：如果第一参数是一个对象，后面又出现了相应的参数配置；这种情况下对象配置优先于
                  后面的参数配置，而且参数的位置也会改变：
                      参数1是对象配置
                    参数2表示 message 及提示文字
                    参数3表示ok按钮的click事件回调函数
                    参数4表示cancel按钮的click事件回调函数
                    参数5表示ok按钮的文本
                    参数6表示cancel按钮的文本内容
                */
                $("#confirm2").click(function () {
                    $.confirm({ message: "is a message", timeout: 3000 }, "message", function () {
                            alert("success");
                        }, function () {
                            alert("failure");
                        }, "按钮");
                });    
                
                /**
                   第一个参数是对象配置，当对象配置中出现的选项会覆盖后面的参数配置
                   $.alert 方法 config 介绍：
                   当第一个参数 是一个对象：
                     {
                         message: arguments[i++] || "default alert message!",
                         onOk: arguments[i++] || _options.emptyHandler(),
                         okText: arguments[i] || "ok"
                    }
                    message 是提示框的提示信息
                    onOk是确定按钮回调函数
                    okText是ok按钮的文字 默认是ok
                    
                  当第一个参数不是一个对象的情况下，那么
                    参数1表示 message 及提示文字
                    参数2表示ok按钮的click事件
                    参数3表示ok按钮的文本
                    
                    注意：如果第一个参数是一个对象，对象中出现的配置：message、onOk、okText，这些配置将会
                    覆盖后面的配置，也就是说这些配置在第一个参数中出现后，后面的参数就不需要
                 */
                $("#alert2").click(function () {
                    $.alert({
                        css: {
                            "background-color": "red"
                        },
                        timeout: 1200,
                        onOk: function () {
                            alert("确定");
                        }
                    }, 
                    "你有一条消息", 
                    function () {
                        alert("被覆盖");
                    }, "yes?");
                });        
                
                var _confirm = function (msg) {
                    $.confirm({
                        message: msg,
                        onOk: function () {
                            alert("你点击了确定按钮");
                        },
                        onCancel: function () {
                            alert("你点击了取消按钮");
                        }
                    });
                };
                $("#confirm3").click(function () {
                    _confirm("message");
                });        
                
                var _alert = function (msg) {
                    $.alert({
                        message: msg,
                        css: {
                            "background-color": "white",
                            "color": "red"
                        },
                        onOk: function () {
                            alert("你点击了确定按钮");
                        }
                    });
                }
                $("#alert3").click(function () {
                    _alert();
                });    
            });
        })(jQuery);
    </script>
    
  </head>
      
  <body>
      <ul>
          <h2>jQuery blockUI Dialog Demos</h2>
          <li>dialog demo <input type="button" value="dialog" id="dialog"/></li>
          <li>confirm callback demos<input type="button" value="confirm" id="confirm"/></li>
          <li>confirm callback demos 2<input type="button" value="confirm" id="confirm2"/></li>
          <li>confirm callback demos 3<input type="button" value="confirm" id="confirm3"/></li>
          <li>alert callback demos<input type="button" value="alert" id="alert"/></li>
          <li>alert callback demos 2<input type="button" value="alert" id="alert2"/></li>
          <li>alert callback demos 3<input type="button" value="alert" id="alert3"/></li>
      </ul>
      
      <div style="display: none;">
          <div id="callback">
              <p>ok or cancel? asdfaf jQuery blockUI Dialog Demos jQuery blockUI Dialog Demos jQuery blockUI Dialog Demos jQuery blockUI Dialog Demos</p>
              <input type="button" value="OK" class="btn ok-btn"/>
              <input type="button" value="Cancel" class="btn cancel-btn"/>
          </div>
      </div>
  </body>
</html>
