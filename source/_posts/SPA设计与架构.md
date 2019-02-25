---
title: SPA设计与架构
date: 2018-10-30 19:57:39
tags: 读书笔记
---

## SPA简述

> 单页Web应用（single page web application，SPA），就是只有一张Web页面的应用，是加载单个HTML 页面并在用户与应用程序交互时动态更新该页面的Web应用程序。
                -- 摘自百度百科

### spa与多页面应用的区别

![spa与多页面应用示意](http://suedar.gz.bcebos.com/spa7.jpg)

在图示中，每个新视图（HTML页面）请求都会导致对服务器端的双向访问。当客户端需要新的数据时，则会向服务器端发送请求。在服务器端，请求由表现层的某个控制器对象拦截。然后该控制器通过服务层与模型层交互，服务层决定完成模型层任务所需的组件。通过数据访问对象（DAO）或服务代理获取数据之后，所有必要的数据更新都将由业务的业务逻辑产生。

控制传回到表现层，在这里选择合适的视图。展示逻辑规定新获取数据在选中视图中如何展示。通常情况下，结果视图是一个包含占位符的源文件，数据（及其他可能的渲染指令）将插入到占位符中。每当控制器将请求路由至视图时，该文件表现得像某种类型得模板，以让视图设置好占位符的数据。

数据与视图整合好之后，视图返回给浏览器，然后浏览器接受新的HTML页面，并通过界面刷新，将包括请求数据的新视图展示给用户。

![在传统web应用程序中，每个新视图（HTML页面）都在服务器端构建](http://suedar.gz.bcebos.com/spa8.jpg)
<center>在传统web应用程序中，每个新视图（HTML页面）都在服务器端构建</center>
<br>

![spa](http://suedar.gz.bcebos.com/spa9.png)
<center>spa应用程序</center>
<br>

转换成人话就是<b>将视图的创建与管理从服务器端解耦出来，并移到UI层。</b>


大家看下这个例子，这是一个典型的spa应用
<p data-height="265" data-theme-id="0" data-slug-hash="oJvWNr" data-default-tab="html,result" data-user="suedar" data-pen-title="Vue SPA" class="codepen">See the Pen <a href="https://codepen.io/suedar/pen/oJvWNr/">Vue SPA</a> by cookie (<a href="https://codepen.io/suedar">@suedar</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

但是当页面进行切换的时候，是没有进行http请求的

![页面切换示例](http://suedar.gz.bcebos.com/spa5.gif)


这是spa应用的典型特点，另外spa应用不利于seo检索。


## MV*框架介绍

在MV*概念中，M代表模型(Model)，V代表视图(View)。

模型 —— 典型的模型包含了数据、业务逻辑以及验证逻辑。
视图 —— 视图即用户所见以及交互的界面，是模型数据的可视化呈现。

### MV*框架

路由到正确视图、整合数据与HTML模板、管理视图生命周期等重任通常委托给被称为MV*（有时被称为SPA框架）的第三方JavaScript文件处理。

单页面应用程序的`单页面`指的是初始HTML页面，或者被称为Shell（外壳页面）。通常情况下，作为用户导航的结果，SPA各个部分按需展示，其中每个部分的HTML骨骼构造被称为模板，模板包含数据占位符。开发者通过框架来绑定数据与模板。

![MV*框架](http://suedar.gz.bcebos.com/sp3.PNG)

### 传统UI设计模式

简述三种设计模式： 模型-视图-控制器（MVC）、 模型-视图-表示器（MVP）、 模型-视图-视图模型（MVVM）。


## 参考

[前端：你要懂的单页面应用和多页面应用](https://juejin.im/post/5a0ea4ec6fb9a0450407725c)
《SPA设计与结构》