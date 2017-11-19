---
layout: post
title: php ImageTTFText()函数GD.. 
date: 2010-11-18 10:33
author: admin
comments: true
categories: []
---

在图片验证码中输入中文是提示：

Warning: imagettftext() [function.imagettftext]: Invalid font filename in   xxxxx

【转】    事实上，出现这个问题，有两种可能：

        一、字体文件书写错误，例如，simhei.ttf写成了simbei.ttf，误差。

        二、路径错误，xp的fonts路径为C:\WINDOWS\Fonts。有一点很重要，那就是看WEB服务器操作系统的类型，若是Linux,或UNIX等等，Fonts的路径应该还会不一样的。

                 如imagettftext($im,10,0,20,20,$bg,'C:\WINDOWS\Fonts\cour.ttf',$str);
        为了防止页面出现乱码，输出前header指定

       //输出图片
header("Content-type:image/jpeg");
ImageJpeg($im,$des_img);

--------------------------------------------

我是在imagettftext（）中，将 simhei.ttf 写成完整路径 c:\WINDOWS\Font\simhei.ttf 后才正确显示的
*.ttf注意大小写
