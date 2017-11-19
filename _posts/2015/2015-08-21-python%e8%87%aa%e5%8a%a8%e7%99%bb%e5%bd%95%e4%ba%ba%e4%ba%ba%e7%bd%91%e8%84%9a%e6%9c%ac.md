---
layout: post
title: python自动登录人人网脚本
date: 2015-08-21 09:09
author: admin
comments: true
categories: []
---
&nbsp;
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">001</td>
<td valign="middle">#!/usr/bin/env python</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">002</td>
<td valign="middle">#encoding=utf-8</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">003</td>
<td valign="middle">importsys</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">004</td>
<td valign="middle">importre</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">005</td>
<td valign="middle">importurllib2</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">006</td>
<td valign="middle">importurllib</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">007</td>
<td valign="middle">importcookielib</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">008</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">009</td>
<td valign="middle">classRenren(object):</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">010</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">011</td>
<td valign="middle">    def__init__(self):</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">012</td>
<td valign="middle">        self.name=self.pwd=self.content=self.domain=self.origURL=  ''</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">013</td>
<td valign="middle">        self.operate=''#登录进去的操作对象</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">014</td>
<td valign="middle">        self.cj=cookielib.LWPCookieJar()</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">015</td>
<td valign="middle">        try:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">016</td>
<td valign="middle">            self.cj.revert('renren.coockie')</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">017</td>
<td valign="middle">        exceptException,e:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">018</td>
<td valign="middle">            printe</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">019</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">020</td>
<td valign="middle">        self.opener=urllib2.build_opener(urllib2.HTTPCookieProcessor(self.cj))</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">021</td>
<td valign="middle">        urllib2.install_opener(self.opener)</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">022</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">023</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">024</td>
<td valign="middle">    defsetinfo(self,username,password,domain,origURL):</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">025</td>
<td valign="middle">        '''设置用户登录信息'''</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">026</td>
<td valign="middle">        self.name=username</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">027</td>
<td valign="middle">        self.pwd=password</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">028</td>
<td valign="middle">        self.domain=domain</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">029</td>
<td valign="middle">        self.origURL=origURL</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">030</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">031</td>
<td valign="middle">    deflogin(self):</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">032</td>
<td valign="middle">        '''登录人人网'''</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">033</td>
<td valign="middle">        params={'domain':self.domain,'origURL':self.origURL,'email':self.name,'password':self.pwd}</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">034</td>
<td valign="middle">        print'login.......'</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">035</td>
<td valign="middle">        req=urllib2.Request(</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">036</td>
<td valign="middle">            'http://www.renren.com/PLogin.do',</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">037</td>
<td valign="middle">            urllib.urlencode(params)</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">038</td>
<td valign="middle">        )</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">039</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">040</td>
<td valign="middle">        self.operate=self.opener.open(req)</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">041</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">042</td>
<td valign="middle">        ifself.operate.geturl()=='http://www.renren.com/Home.do':</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">043</td>
<td valign="middle">            print'Logged on successfully!'</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">044</td>
<td valign="middle">            self.cj.save('renren.coockie')</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">045</td>
<td valign="middle">            self.__viewnewinfo()</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">046</td>
<td valign="middle">        else:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">047</td>
<td valign="middle">            print'Logged on error'</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">048</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">049</td>
<td valign="middle">    def__viewnewinfo(self):</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">050</td>
<td valign="middle">        '''查看好友的更新状态'''</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">051</td>
<td valign="middle">        self.__caiinfo()</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">052</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">053</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">054</td>
<td valign="middle">    def__caiinfo(self):</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">055</td>
<td valign="middle">        '''采集信息'''</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">056</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">057</td>
<td valign="middle">        h3patten=re.compile('&lt;h3&gt;(.*?)&lt;/h3&gt;')#匹配范围</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">058</td>
<td valign="middle">        apatten=re.compile('&lt;a.+&gt;(.+)&lt;/a&gt;:')#匹配作者</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">059</td>
<td valign="middle">        cpatten=re.compile('&lt;/a&gt;(.+)\s')#匹配内容</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">060</td>
<td valign="middle">        infocontent=self.operate.readlines()</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">061</td>
<td valign="middle">#        print infocontent</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">062</td>
<td valign="middle">        print'friend newinfo:'</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">063</td>
<td valign="middle">        foriininfocontent:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">064</td>
<td valign="middle">            content=h3patten.findall(i)</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">065</td>
<td valign="middle">            iflen(content) !=0:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">066</td>
<td valign="middle">                formincontent:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">067</td>
<td valign="middle">                    username=apatten.findall(m)</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">068</td>
<td valign="middle">                    info=cpatten.findall(m)</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">069</td>
<td valign="middle">                    iflen(username) !=0:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">070</td>
<td valign="middle">                        printusername[0],'说:',info[0]</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">071</td>
<td valign="middle">                        print'----------------------------------------------'</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">072</td>
<td valign="middle">                    else:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">073</td>
<td valign="middle">                        continue</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">074</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">075</td>
<td valign="middle">ren=Renren()</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">076</td>
<td valign="middle">username=''#你的人人网的帐号</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">077</td>
<td valign="middle">password=''#你的人人网的密码</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">078</td>
<td valign="middle">domain='renren.com'#人人网的地址</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">079</td>
<td valign="middle">origURL='http://www.renren.com/Home.do'#人人网登录以后的地址</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">080</td>
<td valign="middle">ren.setinfo(username,password,domain,origURL)</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">081</td>
<td valign="middle">ren.login()</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">082</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">083</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">084</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">085</td>
<td valign="middle">主要用到了python cookielib,urllib2,urllib这3个模块，这3个模块是python做http这方面比较好的模块.</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">086</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">087</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">088</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">089</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">090</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">091</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">092</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">093</td>
<td valign="middle">self.cj=cookielib.LWPCookieJar()</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">094</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">095</td>
<td valign="middle">try:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">096</td>
<td valign="middle">    self.cj.revert('renren.coockie')</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">097</td>
<td valign="middle">exceptException,e:</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">098</td>
<td valign="middle">    printe</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">099</td>
<td valign="middle">    self.opener=urllib2.build_opener(urllib2.HTTPCookieProcessor(self.cj))</td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">100</td>
<td valign="middle"></td>
</tr>
</tbody>
</table>
<table cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="middle">101</td>
<td valign="middle">urllib2.install_opener(self.opener)</td>
</tr>
</tbody>
</table>
