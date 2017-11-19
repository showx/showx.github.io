---
layout: post
title: Nginx CORS实现JS跨域
date: 2015-06-11 16:52
author: admin
comments: true
categories: []
---
1. 什么是跨域
简单地理解就是因为JavaScript同源策略的限制，a.com 域名下的js无法操作b.com或是c.a.com域名下的对象。

同源是指相同的协议、域名、端口。特别注意两点：

如果是协议和端口造成的跨域问题“前台”是无能为力的，
在跨域问题上，域仅仅是通过“协议+域名+端口”来识别，两个不同的域名即便指向同一个ip地址，也是跨域的。
2. 跨域解决方案
跨域解决方案有多种，大多是利用JS Hack：

document.domain+iframe的设置
动态创建script
利用iframe和location.hash
window.name实现的跨域数据传输
使用HTML5 postMessage
利用flash
以上方案见http://www.cnblogs.com/rainman/archive/2011/02/20/1959325.html#m5
通过代理，js访问代理，代理转到不同的域http://developer.yahoo.com/javascript/howto-proxy.html
Jquery JSONP(本质上就是动态创建script)http://www.cnblogs.com/chopper/archive/2012/03/24/2403945.html
跨域资源共享（CORS） 这就是我们要介绍的跨域解决方案，也是未来的跨域问题的标准解决方案
3. CORS
CORS: 跨域资源共享(Cross-Origin Resource Sharing)http://www.w3.org/TR/cors/

当前几乎所有的浏览器（Internet Explorer 8+， Firefox 3.5+， Safari 4+和 Chrome 3+）都可通过名为跨域资源共享（Cross-Origin Resource Sharing）的协议支持ajax跨域调用。(see: http://caniuse.com/#search=cors)

Chrome, Firefox, Opera and Safari 都使用的是 XMLHttpRequest2 对象， IE使用XDomainRequest。XMLHttpRequest2的Request属性：open()、setRequestHeader()、timeout、withCredentials、upload、send()、send()、abort()。

XMLHttpRequest2的Response属性：status、statusText、getResponseHeader()、getAllResponseHeaders()、entity、overrideMimeType()、responseType、response、responseText、responseXML。

启用 CORS 请求

假设您的应用已经在 example.com 上了，而您想要从 www.example2.com 提取数据。一般情况下，如果您尝试进行这种类型的 AJAX 调用，请求将会失败，而浏览器将会出现“源不匹配”的错误。利用 CORS，www.example2.com 服务端只需添加一个HTTP Response头，就可以允许来自 example.com 的请求：

Access-Control-Allow-Origin: http://example.com
Access-Control-Allow-Credentials: true（可选）
可将 Access-Control-Allow-Origin 添加到某网站下或整个域中的单个资源。要允许任何域向您提交请求，请设置如下：

Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true（可选）
其实，该网站 (html5rocks.com) 已在其所有网页上均启用了 CORS。启用开发人员工具后，您就会在我们的响应中看到 Access-Control-Allow-Origin 了。

提交跨域请求

如果服务器端已启用了 CORS，那么提交跨域请求就和普通的 XMLHttpRequest 请求没什么区别。例如，现在 example.com 可以向 www.example2.com 提交请求了：

var xhr = new XMLHttpRequest();
// xhr.withCredentials = true; //如果需要Cookie等
xhr.open('GET', 'http://www.example2.com/hello.json');
xhr.onload = function(e) {
  var data = JSON.parse(this.response);
  ...
}
xhr.send();
4. 服务端Nginx配置
要实现CORS跨域，服务端需要这个一个流程:http://www.html5rocks.com/static/images/cors_server_flowchart.png

对于简单请求，如GET，只需要在HTTP Response后添加Access-Control-Allow-Origin。

对于非简单请求，比如POST、PUT、DELETE等，浏览器会分两次应答。第一次preflight（method: OPTIONS），主要验证来源是否合法，并返回允许的Header等。第二次才是真正的HTTP应答。所以服务器必须处理OPTIONS应答。

http://enable-cors.org/server_nginx.html这里是一个nginx启用COSR的参考配置。

流程如下：

首先查看http头部有无origin字段；
如果没有，或者不允许，直接当成普通请求处理，结束；
如果有并且是允许的，那么再看是否是preflight(method=OPTIONS)；
如果是preflight，就返回Allow-Headers、Allow-Methods等，内容为空；
如果不是preflight，就返回Allow-Origin、Allow-Credentials等，并返回正常内容。
用伪代码表示：

location /pub/(.+) {
    if ($http_origin ~ <允许的域（正则匹配）>) {
        add_header 'Access-Control-Allow-Origin' "$http_origin";
        add_header 'Access-Control-Allow-Credentials' "true";
        if ($request_method = "OPTIONS") {
            add_header 'Access-Control-Max-Age' 86400;
            add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE';
            add_header 'Access-Control-Allow-Headers' 'reqid, nid, host, x-real-ip, x-forwarded-ip, event-type, event-id, accept, content-type';
            add_header 'Content-Length' 0;
            add_header 'Content-Type' 'text/plain, charset=utf-8';
            return 204;
        }
    }
    # 正常nginx配置
    ......
}
但是由于Nginx 的 if 是邪恶的，所以配置就相当地让人不爽（是我的配置不够简洁吗？）。下面nginx-spdy-push里/pub接口启用CORS的配置：

# push publish
# broadcast channel name must start with '_'
# (i.e., normal channel must not start with '_')

# GET    /pub/channel_id -> get statistics about a channel
# POST   /pub/channel_id -> publish a message to the channel
# DELETE /pub_admin?id=channel_id -> delete the channel

#rewrite_log on;

# server_name test.gw.com.cn
# listen      2443 ssl spdy

location ~ ^/pub/([-_.A-Za-z0-9]+)$ {

    set $cors "local";

    # configure CORS based on https://gist.github.com/alexjs/4165271
    # (See: http://www.w3.org/TR/2013/CR-cors-20130129/#access-control-allow-origin-response-header )

    if ( $http_origin ~* "https://.+\.gw\.com\.cn(?=:[0-9]+)?" ) {
        set $cors "allow";
    }
    if ($request_method = "OPTIONS") {
        set $cors "${cors}options";
    }

    # if CORS request is not a simple method
    if ($cors = "allowoptions") {
        # Tells the browser this origin may make cross-origin requests
        add_header 'Access-Control-Allow-Origin' "$http_origin";
        # in a preflight response, tells browser the subsequent actual request can include user credentials (e.g., cookies)
        add_header 'Access-Control-Allow-Credentials' "true";

        # === Return special preflight info ===

        # Tell browser to cache this pre-flight info for 1 day
        add_header 'Access-Control-Max-Age' 86400;

        # Tell browser we respond to GET,POST,OPTIONS in normal CORS requests.
        # Not officially needed but still included to help non-conforming browsers.
        # OPTIONS should not be needed here, since the field is used
        # to indicate methods allowed for 'actual request' not the preflight request.
        # GET,POST also should not be needed, since the 'simple methods' GET,POST,HEAD are included by default.
        # We should only need this header for non-simple requests  methods (e.g., DELETE), or custom request methods (e.g., XMODIFY)
        add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE';

        # Tell browser we accept these headers in the actual request
        add_header 'Access-Control-Allow-Headers' 'reqid, nid, host, x-real-ip, x-forwarded-ip, event-type, event-id, accept, content-type';

        # === response for OPTIONS method ===

        # no body in this response
        add_header 'Content-Length' 0;
        # (should not be necessary, but included for non-conforming browsers)
        add_header 'Content-Type' 'text/plain, charset=utf-8';
        # indicate successful return with no content
        return 204;
    }

    if ($cors = "allow") {
        rewrite /pub/(.*) /pub_cors/$1 last;
    }

    if ($cors = "local") {
        rewrite /pub/(.*) /pub_int/$1 last;
    }
}

location ~ /pub_cors/(.*) {
    internal;
    # Tells the browser this origin may make cross-origin requests
    add_header 'Access-Control-Allow-Origin' "$http_origin";
    # in a preflight response, tells browser the subsequent actual request can include user credentials (e.g., cookies)
    add_header 'Access-Control-Allow-Credentials' "true";

    push_stream_publisher                   admin; # enable delete channel
    set $push_stream_channel_id             $1;

    push_stream_store_messages              on;  # enable /sub/ch.b3
    push_stream_channel_info_on_publish     on;
}

location ~ /pub_int/(.*) {
  #  internal;
    push_stream_publisher                   admin; # enable delete channel
    set $push_stream_channel_id             $1;

    push_stream_store_messages              on;  # enable /sub/ch.b3
    push_stream_channel_info_on_publish     on;
}
5. 客户端javascript代码
下面是https://spdy.gw.com.cn/sse.html里的代码

var xhr = new XMLHttpRequest();
//xhr.withCredentials = true;
xhr.open("POST", "https://test.gw.com.cn:2443/pub/ch1", true);
// xhr.setRequestHeader("accept", "application/json");

xhr.onload = function()  {
  $('back').innerHTML = xhr.responseText;
  $('ch1').value = + $('ch1').value + 1;
}
xhr.onerror = function() {
  alert('Woops, there was an error making the request.');
};

xhr.send($('ch1').value);
页面的域是https://spdy.gw.com.cn， XMLHttpRequest的域是https://test.gw.com.cn:2443，不同的域，并且Post方式。

用Chrome测试，可以发现有一次OPTIONS应答。如过没有OPTIONS应答，可能是之前已经应答过，被浏览器缓存了'Access-Control-Max-Age' 86400;，清除缓存，再试验。

6. 相关链接
Using CORS
http://www.html5rocks.com/en/tutorials/cors/
XMLHttpRequest2草案
http://www.w3.org/TR/XMLHttpRequest2/
XMLHttpRequest2 新技巧
http://www.html5rocks.com/zh/tutorials/file/xhr2/
XMLHttpRequest Level 2 使用指南
http://www.ruanyifeng.com/blog/2012/09/xmlhttprequest_level_2.html
XMLHttpRequest2的进步之处
http://mozilla.com.cn/post/34886/
理解DOMString、Document、FormData、Blob、File、ArrayBuffer数据类型
http://www.css119.com/archives/1595
nginx启用COSR的参考配置
http://enable-cors.org/server_nginx.html
要实现CORS跨域，服务端需要这个一个流程
http://www.html5rocks.com/static/images/cors_server_flowchart.png
Compatibility tables for support of HTML5, CSS3, SVG and more in desktop and mobile browsers
http://caniuse.com/#search=cors
通过代理，js访问代理，代理转到不同的域
http://developer.yahoo.com/javascript/howto-proxy.html
JavaScript跨域总结与解决办法
http://www.cnblogs.com/rainman/archive/2011/02/20/1959325.html#m5
深入浅出JSONP--解决ajax跨域问题
http://www.cnblogs.com/chopper/archive/2012/03/24/2403945.html
JavaScript: Use a Web Proxy for Cross-Domain XMLHttpRequest Calls
http://developer.yahoo.com/javascript/howto-proxy.html
