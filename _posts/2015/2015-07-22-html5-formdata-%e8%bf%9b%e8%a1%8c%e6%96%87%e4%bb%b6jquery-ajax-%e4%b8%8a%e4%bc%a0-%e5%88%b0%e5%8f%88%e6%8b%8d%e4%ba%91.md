---
layout: post
title: HTML5 FormData 进行文件jquery ajax 上传 到又拍云
date: 2015-07-22 10:06
author: admin
comments: true
categories: []
---
页面test.html
<div id="" class="dp-highlighter">
<div class="bar">
<div class="tools">Html代码 <embed src="http://ileson.iteye.com/javascripts/syntaxhighlighter/clipboard_new.swf" type="application/x-shockwave-flash" width="14" height="15"></embed> <a title="收藏这段代码"><img class="star" src="http://ileson.iteye.com/images/icon_star.png" alt="收藏代码" /></a></div>
</div>
<ol class="dp-xml" start="1">
	<li>&lt;!DOCTYPE<span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">html</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">head</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">title</span><span class="tag">&gt;</span> formdata file jquery ajax upload<span class="tag">&lt;/</span><span class="tag-name">title</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">head</span><span class="tag">&gt;</span></li>
	<li></li>
	<li><span class="tag">&lt;</span><span class="tag-name">body</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">form</span> <span class="attribute">class</span>=<span class="attribute-value">"form-horizontal"</span> <span class="attribute">role</span>=<span class="attribute-value">"form"</span> <span class="attribute">id</span>=<span class="attribute-value">"myForm"</span></li>
	<li>                    <span class="attribute">action</span>=<span class="attribute-value">"http://v0.api.upyun.com/xxx"</span> <span class="attribute">method</span>=<span class="attribute-value">"post"</span></li>
	<li>                    <span class="attribute">enctype</span>=<span class="attribute-value">"multipart/form-data"</span><span class="tag">&gt;</span></li>
	<li></li>
	<li>                    <span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">type</span>=<span class="attribute-value">"hidden"</span> <span class="attribute">name</span>=<span class="attribute-value">"policy"</span></li>
	<li>                        <span class="attribute">value</span>=<span class="attribute-value">""</span><span class="tag">&gt;</span></li>
	<li>                    <span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">type</span>=<span class="attribute-value">"hidden"</span> <span class="attribute">name</span>=<span class="attribute-value">"signature"</span></li>
	<li>                        <span class="attribute">value</span>=<span class="attribute-value">""</span><span class="tag">&gt;</span></li>
	<li></li>
	<li>                    <span class="tag">&lt;</span><span class="tag-name">div</span> <span class="attribute">class</span>=<span class="attribute-value">"form-group"</span><span class="tag">&gt;</span></li>
	<li>                        <span class="tag">&lt;</span><span class="tag-name">label</span> <span class="attribute">class</span>=<span class="attribute-value">"col-sm-2 control-label"</span><span class="tag">&gt;</span>说明:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li>                        <span class="tag">&lt;</span><span class="tag-name">div</span> <span class="attribute">class</span>=<span class="attribute-value">"col-sm-10"</span><span class="tag">&gt;</span></li>
	<li>                            <span class="tag">&lt;</span><span class="tag-name">p</span> <span class="attribute">class</span>=<span class="attribute-value">"form-control-static "</span><span class="tag">&gt;</span>ajax 文件上传 。<span class="tag">&lt;/</span><span class="tag-name">p</span><span class="tag">&gt;</span></li>
	<li>                        <span class="tag">&lt;/</span><span class="tag-name">div</span><span class="tag">&gt;</span></li>
	<li>                    <span class="tag">&lt;/</span><span class="tag-name">div</span><span class="tag">&gt;</span></li>
	<li>                    <span class="tag">&lt;</span><span class="tag-name">div</span> <span class="attribute">class</span>=<span class="attribute-value">"form-group"</span><span class="tag">&gt;</span></li>
	<li>                        <span class="tag">&lt;</span><span class="tag-name">label</span> <span class="attribute">for</span>=<span class="attribute-value">"url"</span> <span class="attribute">class</span>=<span class="attribute-value">"col-sm-2 control-label"</span><span class="tag">&gt;</span><span class="tag">&lt;</span><span class="tag-name">s</span><span class="tag">&gt;</span>*<span class="tag">&lt;/</span><span class="tag-name">s</span><span class="tag">&gt;</span>图片选择:<span class="tag">&lt;/</span><span class="tag-name">label</span><span class="tag">&gt;</span></li>
	<li>                        <span class="tag">&lt;</span><span class="tag-name">div</span> <span class="attribute">class</span>=<span class="attribute-value">"col-sm-7"</span><span class="tag">&gt;</span></li>
	<li>                            <span class="tag">&lt;</span><span class="tag-name">input</span> <span class="attribute">type</span>=<span class="attribute-value">"file"</span> <span class="attribute">name</span>=<span class="attribute-value">"file"</span> <span class="attribute">id</span>=<span class="attribute-value">"file_upload"</span> <span class="attribute">value</span>=<span class="attribute-value">""</span></li>
	<li>                                <span class="attribute">class</span>=<span class="attribute-value">"form-control"</span> <span class="attribute">placeholder</span>=<span class="attribute-value">"图片地址"</span><span class="tag">&gt;</span></li>
	<li>                        <span class="tag">&lt;/</span><span class="tag-name">div</span><span class="tag">&gt;</span></li>
	<li>                    <span class="tag">&lt;/</span><span class="tag-name">div</span><span class="tag">&gt;</span></li>
	<li></li>
	<li>                    <span class="tag">&lt;</span><span class="tag-name">div</span> <span class="attribute">class</span>=<span class="attribute-value">"form-group"</span><span class="tag">&gt;</span></li>
	<li>                        <span class="tag">&lt;</span><span class="tag-name">div</span> <span class="attribute">class</span>=<span class="attribute-value">"col-sm-offset-2 col-sm-7"</span><span class="tag">&gt;</span></li>
	<li></li>
	<li>                            <span class="tag">&lt;</span><span class="tag-name">a</span> <span class="attribute">id</span>=<span class="attribute-value">"btnAjax"</span> <span class="attribute">class</span>=<span class="attribute-value">"btn btn-info col-md-5 col-lg-offset-1"</span></li>
	<li>                                <span class="attribute">onclick</span>=<span class="attribute-value">"uploadByForm();"</span><span class="tag">&gt;</span>Ajax上传<span class="tag">&lt;/</span><span class="tag-name">a</span><span class="tag">&gt;</span></li>
	<li>                        <span class="tag">&lt;/</span><span class="tag-name">div</span><span class="tag">&gt;</span></li>
	<li>                    <span class="tag">&lt;/</span><span class="tag-name">div</span><span class="tag">&gt;</span></li>
	<li>                <span class="tag">&lt;/</span><span class="tag-name">form</span><span class="tag">&gt;</span></li>
	<li></li>
	<li><span class="tag">&lt;</span><span class="tag-name">script</span> <span class="attribute">src</span>=<span class="attribute-value">"http://cdn.bootcss.com/jquery/1.11.1/jquery.min.js"</span><span class="tag">&gt;</span><span class="tag">&lt;/</span><span class="tag-name">script</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;</span><span class="tag-name">script</span> <span class="attribute">type</span>=<span class="attribute-value">"text/javascript"</span><span class="tag">&gt;</span></li>
	<li></li>
	<li>/**</li>
	<li>         * ajax 上传。</li>
	<li>         */</li>
	<li>        function uploadByForm() {</li>
	<li>                var <span class="attribute">formData</span> = <span class="attribute-value">new</span> FormData($("#myForm")[0]);//用form 表单直接 构造formData 对象; 就不需要下面的append 方法来为表单进行赋值了。</li>
	<li></li>
	<li>            //var <span class="attribute">formData</span> = <span class="attribute-value">new</span> FormData();//构造空对象，下面用append 方法赋值。</li>
	<li>//          formData.append("policy", "");</li>
	<li>//          formData.append("signature", "");</li>
	<li>//          formData.append("file", $("#file_upload")[0].files[0]);</li>
	<li>            var <span class="attribute">url</span> = <span class="attribute-value">"http://v0.api.upyun.com/xxx"</span>;</li>
	<li>            $.ajax({</li>
	<li>                url : url,</li>
	<li>                type : 'POST',</li>
	<li>                data : formData,</li>
	<li></li>
	<li>                /**</li>
	<li>                 * 必须false才会避开jQuery对 formdata 的默认处理</li>
	<li>                 * XMLHttpRequest会对 formdata 进行正确的处理</li>
	<li>                 */</li>
	<li>                processData : false,</li>
	<li>                /**</li>
	<li>                 *必须false才会自动加上正确的Content-Type</li>
	<li>                 */</li>
	<li>                contentType : false,</li>
	<li>                success : function(responseStr) {</li>
	<li>                                    alert("成功：" + JSON.stringify(responseStr));</li>
	<li>                    //                  var <span class="attribute">jsonObj</span> = $.parseJSON(responseStr);//eval("("+responseStr+")");</li>
	<li>                },</li>
	<li>                error : function(responseStr) {</li>
	<li>                    alert("失败:" + JSON.stringify(responseStr));//将    json对象    转成    json字符串。</li>
	<li>                }</li>
	<li>            });</li>
	<li>        }</li>
	<li><span class="tag">&lt;/</span><span class="tag-name">script</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">body</span><span class="tag">&gt;</span></li>
	<li><span class="tag">&lt;/</span><span class="tag-name">html</span><span class="tag">&gt;</span></li>
</ol>
</div>
