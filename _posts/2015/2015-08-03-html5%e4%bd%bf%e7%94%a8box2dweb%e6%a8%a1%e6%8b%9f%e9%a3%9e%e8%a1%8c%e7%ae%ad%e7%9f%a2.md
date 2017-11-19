---
layout: post
title: [HTML5]使用Box2dWeb模拟飞行箭矢
date: 2015-08-03 10:30
author: admin
comments: true
categories: []
---
Box2d是一个2D游戏物理引擎，由Erin Catto开发，于2007年发布。很多2D游戏都用过Box2d，其中最有名的自然是愤怒的小鸟。Box2d本身是C++编写，但在不同平台都有它的衍生版本，像Flash版的Box2dFlash，JS版的Box2dJS和Box2dWeb。最近偶然看到一篇使用Box2dFlash模拟箭矢飞行效果的文章：
http://www.emanueleferonato.com/2012/12/10/flying-arrows-simulation-with-box2d/
很有意思，想尝试使用下Box2d。
之前从没接触过Flash，选择JS版的Box2d，而Box2dJS已经很久没更新，所以使用Box2dWeb重写箭矢飞行效果。
网上有不少Box2d教程，不过介绍其应用的多些。对于Box2d基本概念和原理，推荐阿蕉的博客，他将Box2d C++的系列教程译成中文。虽然C++和JS不同，但是Box2d原理是相通的，可以参考。
http://blog.csdn.net/wen294299195/article/category/1227604
首先下载Box2dWeb
https://code.google.com/p/box2dweb/downloads/list
压缩包里只有四个文件，这里只需要Box2dWeb-2.1.a.3.min.js（也可以用Box2dWeb-2.1.a.3.js，方便了解Box2DWeb的各个函数）。
按照下面的目录结构创建各个文件即可：
|-js/
| |-Box2dWeb-2.1.a.3.min.js
| |-game.js
|-arrow.html
编辑arrow.html，引用javascript文件并创建一个canvas标签，代码如下：
[html] view plaincopyprint?
<!DOCTYPE HTML>  
<html>  
    <head>  
       <title>Box2DWeb Test</title>  
        <scripttypescripttype="text/javascript"src="js/Box2dWeb-2.1.a.3.min.js"></script>  
        <scripttypescripttype="text/javascript" src="js/game.js"></script>  
    </head>  
   
    <bodyonloadbodyonload="init();">  
        <canvasidcanvasid="canvas" width="640" height="480"style="background-color:#333333;"></canvas>  
    </body>  
</html>  
接下来编辑game.js。从arrow.html中可以看到，页面载入后调用init方法模拟整个过程，添加下面的代码：
[javascript] view plaincopyprint?
function init() {  
// Commen code for usingBox2D object.  
    var b2Vec2 =Box2D.Common.Math.b2Vec2;  
    var b2AABB =Box2D.Collision.b2AABB;  
    var b2BodyDef =Box2D.Dynamics.b2BodyDef;  
    var b2Body =Box2D.Dynamics.b2Body;  
    varb2FixtureDef = Box2D.Dynamics.b2FixtureDef;  
    var b2Fixture =Box2D.Dynamics.b2Fixture;  
    var b2World =Box2D.Dynamics.b2World;  
    varb2PolygonShape = Box2D.Collision.Shapes.b2PolygonShape;  
var b2DebugDraw =Box2D.Dynamics.b2DebugDraw;  
/* 下文添加 */  
};  
以上代码是为了方便使用Box2d中的对象。接着就是设置全局属性，像canvas的大小、Box2d的世界参数、鼠标消息响应方法等。
[javascript] view plaincopyprint?
// Get canvas fordrawing.  
var canvas =document.getElementById("canvas");  
var canvasPosition =getElementPosition(canvas);  
var context =canvas.getContext("2d");  
     
// World constants.  
var worldScale = 30;  
var dragConstant=0.05;  
var dampingConstant = 2;  
var world = newb2World(new b2Vec2(0, 10),true);  
     
document.addEventListener("mousedown",onMouseDown);  
debugDraw();              
window.setInterval(update,1000/60);  
设置好Box2d的世界之后就可以放置各种模型。接下来的代码创建四面墙壁来封闭区域：
[javascript] view plaincopyprint?
// Create bottom wall  
createBox(640,30,320,480,b2Body.b2_staticBody,null);  
// Create top wall  
createBox(640,30,320,0,b2Body.b2_staticBody,null);  
// Create left wall  
createBox(30,480,0,240,b2Body.b2_staticBody,null);  
// Create right wall  
createBox(30,480,640,240,b2Body.b2_staticBody,null);  
   
functioncreateBox(width,height,pX,pY,type,data){  
    var bodyDef = new b2BodyDef;  
    bodyDef.type = type;  
    bodyDef.position.Set(pX/worldScale,pY/worldScale);  
    bodyDef.userData=data;  
   
    var polygonShape = new b2PolygonShape;  
    polygonShape.SetAsBox(width/2/worldScale,height/2/worldScale);  
   
    var fixtureDef = new b2FixtureDef;  
    fixtureDef.density = 1.0;  
    fixtureDef.friction = 0.5;  
    fixtureDef.restitution = 0.5;  
    fixtureDef.shape = polygonShape;  
         
    var body=world.CreateBody(bodyDef);  
    body.CreateFixture(fixtureDef);  
}  
希望点击鼠标时，从左下角向鼠标点击的位置发射一支箭。所以在鼠标点击消息的响应方法中调用createArrow方法，根据传入的坐标生成一支箭，并赋给它初速度，代码如下：
[javascript] view plaincopyprint?
function onMouseDown(e) {  
    var evt = e||window.event;  
    createArrow(e.clientX-canvasPosition.x,e.clientY-canvasPosition.y);  
}  
   
function createArrow(pX,pY) {  
    // Set the left corner as the originalpoint.  
    var angle = Math.atan2(pY-450, pX);  
   
    // Define the shape of arrow.  
    var vertices = [];  
    vertices.push(new b2Vec2(-1.4,0));  
    vertices.push(new b2Vec2(0,-0.1));  
    vertices.push(new b2Vec2(0.6,0));  
    vertices.push(newb2Vec2(0,0.1));  
   
    var bodyDef = new b2BodyDef;  
    bodyDef.type = b2Body.b2_dynamicBody;  
    bodyDef.position.Set(40/worldScale,400/worldScale);  
    bodyDef.userData = "Arrow";  
   
    var polygonShape = new b2PolygonShape;  
    polygonShape.SetAsVector(vertices,4);  
   
    var fixtureDef = new b2FixtureDef;  
    fixtureDef.density = 1.0;  
    fixtureDef.friction = 0.5;  
    fixtureDef.restitution = 0.5;  
    fixtureDef.shape = polygonShape;  
         
    var body = world.CreateBody(bodyDef);  
    body.CreateFixture(fixtureDef);  
   
    // Set original state of arrow.  
    body.SetLinearVelocity(newb2Vec2(20*Math.cos(angle), 20*Math.sin(angle)));  
    body.SetAngle(angle);  
    body.SetAngularDamping(dampingConstant);  
 }  
接下来就是系统方法，绘制物体并定时更新：
[javascript] view plaincopyprint?
function debugDraw() {  
    var debugDraw = new b2DebugDraw();  
    debugDraw.SetSprite(document.getElementById("canvas").getContext("2d"));  
    debugDraw.SetDrawScale(worldScale);  
    debugDraw.SetFillAlpha(0.5);  
    debugDraw.SetLineThickness(1.0);  
    debugDraw.SetFlags(b2DebugDraw.e_shapeBit | b2DebugDraw.e_jointBit);  
    world.SetDebugDraw(debugDraw);  
}  
      
function update() {   
    world.Step(1/60,10,10);  
    world.ClearForces();  
  
    for(var b = world.m_bodyList; b != null; b = b.m_next){  
       if(b.GetUserData() === "Arrow") {  
                updateArrow(b);  
            }  
    }  
          
    world.DrawDebugData();  
}  
注意上面update方法中调用的updateArrow方法，它负责模拟箭矢在空中运动形态，让整个过程更加真实。
[javascript] view plaincopyprint?
functionupdateArrow(arrowBody) {  
    // Calculate arrow's fligth speed.  
    var flightSpeed =Normalize2(arrowBody.GetLinearVelocity());  
   
    // Calculate arrow's pointingdirection.  
    var bodyAngle = arrowBody.GetAngle();  
    var pointingDirection = new b2Vec2(Math.cos(bodyAngle),-Math.sin(bodyAngle));  
   
    // Calculate arrow's flightingdirection and normalize it.  
    var flightAngle =Math.atan2(arrowBody.GetLinearVelocity().y,arrowBody.GetLinearVelocity().x);  
    var flightDirection = newb2Vec2(Math.cos(flightAngle), Math.sin(flightAngle));  
   
    // Calculate dot production.  
    var dot = b2Dot( flightDirection,pointingDirection );  
    var dragForceMagnitude = (1 -Math.abs(dot)) * flightSpeed * flightSpeed * dragConstant *arrowBody.GetMass();  
    var arrowTailPosition =arrowBody.GetWorldPoint(new b2Vec2( -1.4, 0 ) );  
    arrowBody.ApplyForce( newb2Vec2(dragForceMagnitude*-flightDirection.x,dragForceMagnitude*-flightDirection.y),arrowTailPosition );  
}  
   
function b2Dot(a, b) {  
    return a.x * b.x + a.y * b.y;  
}  
   
function Normalize2(b) {  
    return Math.sqrt(b.x * b.x + b.y *b.y);  
}  
最后是getElementPosition方法，用于获得canvas的偏移坐标：
[javascript] view plaincopyprint?
//http://js-tut.aardon.de/js-tut/tutorial/position.html  
function getElementPosition(element) {  
    var elem=element, tagname="",x=0, y=0;  
    while((typeof(elem) =="object") && (typeof(elem.tagName) != "undefined")){  
        y += elem.offsetTop;  
        x += elem.offsetLeft;  
        tagname = elem.tagName.toUpperCase();  
        if(tagname == "BODY"){  
            elem=0;  
        }  
        if(typeof(elem) =="object"){  
            if(typeof(elem.offsetParent) =="object"){  
                elem = elem.offsetParent;  
            }  
        }  
    }  
    return {x: x, y: y};  
}  
