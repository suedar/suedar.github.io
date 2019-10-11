---
title: 瞅一眼grid(弃文)
date: 2019-10-11 18:21:13
tags:
---

> grid属性总记不住，故总结此文，代码戳图片标题看哦～

首先看一个栗子🌰

![博客](https://blog1.cdn.bcebos.com/grid%2Fa.png)
<b><center>图1 🌰</center></b>

这是我之前做的博客，数据是mock的，anyway，在`grid网格布局`出现以前，假如我们要做这么一个博客，是要考虑非常多的布局因素的，这个页面是明显的`header + content + footer`的一个布局（footer没截到），而`content区域`是一个三栏布局。假如内容区原本通过固定宽度并`margin居中`的话，右侧的推荐栏要加上是非常麻烦的，但是在`grid布局`中实现就非常便利啦，只要在子元素设置`grid-column`即可。

<p class="codepen" data-height="265" data-theme-id="0" data-default-tab="css,result" data-user="suedar" data-slug-hash="yLLYGbG" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="grid">
  <span>See the Pen <a href="https://codepen.io/suedar/pen/yLLYGbG">
  grid</a> by cookie (<a href="https://codepen.io/suedar">@suedar</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

是时候学习`grid`了~

# grid

![grid](https://blog1.cdn.bcebos.com/grid%2Fc.png)
<b><center>图3 Grid属性汇总</center></b>


`grid网格布局`是一种二维布局，其将页面划分为一个个网格，通过行(`column`)和列(`row`)的巧妙组合，构成响应式的页面。


![网格](https://blog1.cdn.bcebos.com/grid%2Fb.png)
<b><center>图4 网格</center></b>

截止2017年，大多数浏览器都已经支持这个布局。（想康康兼容性戳[这里](https://caniuse.com/#search=grid)）

grid有作用在容器上以及子元素上面的属性。

阮大大写的真详细 弃文。。。

> [CSS Grid 布局完全指南(图解 Grid 详细教程)](https://www.html.cn/archives/8510/)<br>
> [grid的一个示例](https://codepen.io/mor10/pen/NjeqyX)<br>
> [CSS Grid 网格布局教程](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)