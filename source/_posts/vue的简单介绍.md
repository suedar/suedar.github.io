<!--
 * @file: 
 * @author: sunchao(sunchao15@baidu.com)
 * @Date: 2019-08-05 14:17:01
 * @LastEditors: sunchao
 * @LastEditTime: 2019-08-11 14:32:37
 -->
---
title: 前端历史小记（该文作废）
date: 2019-08-05 14:17:01
tags:
---

各位同学大家好，本次分享的主要内容是关于前端历史的简单介绍，仅适用于前端零基础的初学者。

### 洪荒时期（1989～2006）
首先我们需要知道的是，一个合格的网页是由什么构成的呢？
答案是`html`、`css`和`javascript`，这三者分管网页的结构，样式和行为，并称为网页三要素。其中`HTML`决定了网页的结构和内容，`CSS`用来设定了网页的表现样式，`javascript`用来控制网页的行为。本文主要讲述`JavaScript`的发展。`JavaScript`是一种处理网页的脚本语言。尽管它的名字和`Java`类似，但是它是由网景公司开发的而不是由太阳计算机系统公司开发的，除了两者的语法都是从C语言发展而来这一点外，它们之间几乎没有什么关系。之所以叫`JavaScript`，只是当时网景公司希望能借助`Java`的名气推广它, 也就是传说中的蹭热点～由于网页浏览器中，`JavaScript`与文档对象模型（`Document Object Model`）紧密结合，能够很好地处理网页，使得它比它的作者原本预期的要有用得多。它的用途可以用术语`DHTML`（动态`HTML`）表达，以强调它和静态`HTML`网页的区别。

而`ECMAScript`是`JavaScript`的标准版本，`es`是`ECMAScript`的简称。由于JavaScript是个在10天创立出来的语言，所以，不可避免地有诸多纰漏，在后续的版本迭代中，我们希望去逐渐修复它。在2015年`es6`发布，2016`es7`发布，以此类推。

英国科学家蒂姆·伯纳斯-李于1989年发明了万维网。而后网景公司发布了第一款商业浏览器`Navigator`。
![（1994 年，网景浏览器的截图）](https://pic3.zhimg.com/80/v2-ed02785797334fa618a9d8abbe0e09aa_hd.jpg)
<center>（1994 年，网景浏览器的截图）</center>

而后爆发了浏览器大战，第一时期为1998年微软公司的Internet Explorer取代网景公司的Netscape Navigator成为主要浏览器。第二时期为2003年后Internet Explorer份额逐渐受到其他浏览器蚕食，包括Mozilla Firefox，Google Chrome，Safari和Opera。若是众多浏览器按照不同的标准开发的话，前端会有繁复的兼容性问题。

比如我们要写一个事件绑定的函数，兼容所有浏览器。（该方法称为`特征检测`）
``` js
// 该方法来自《JavaScript高级程序设计》
var eventUtil = {
    add: function(el, type, handler) {
        if (el.addEventListener) { // DOM2
            el.addEventListener(type, handler, false);
        } else if (el.attachEvent) {
            el.attachEvent('on' + type, handler); // ie
        } else {
            el['on' + type] = handler; // DOM0
        }
    },
    off: function(el, type, handler) {
        if (el.removeEventListener) {
            el.removeEventListener(type, handler, false)
        } else if (el.detachEvent) {
            el.detachEvent(type, handler);
        } else {
            el['on' + type] = null;
        }
    }
};
```

### jQuery时期（2006～2016）

我们知道，JavaScript是一门解释性语言，执行速度要比编译型语言慢得多，所以性能优化尤其重要。2006年到2016年，那是jquery的时代，其声称能提升性能并解决浏览器兼容性问题。

但是，随着硬件设备和浏览器的发展，页面展示从原来的静态页面展示变得越来越多样化，需求在增长，时代在变革，一个页面需要依赖的模块越来越多，于是会引入十多个乃至几十个jQuery插件，页面上塞满了Script标签。众所周知，浏览器是单线程，Script的加载，会影响到页面的解析与呈现，导致著名的白屏问题。jQuery另一个问题是全局污染，由于插件的质量问题，或者开发的素质问题，这已经是IIEF模块或命名空间等传统手段无法解决了。（[《前端开发 20 年变迁史》](https://zhuanlan.zhihu.com/p/68030183)）

### 三大框架时期（2016～至今）
那么，怎么解决模块越来越多的问题呢？模块层出不穷，模块管理工具亦如雨后春笋般出现，现在成为主流的



> https://zh.wikipedia.org/wiki/%E6%B5%8F%E8%A7%88%E5%99%A8%E5%A4%A7%E6%88%98<br>
> https://zhuanlan.zhihu.com/p/68030183