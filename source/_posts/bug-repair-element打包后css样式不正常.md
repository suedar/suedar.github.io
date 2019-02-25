---
title: '[bug repair]element打包后css样式不正常'
date: 2019-02-25 21:37:51
tags:
---

element打包后出现css样式与开发不一样的问题

会导致这个结果的原因是 element设定的样式把用户写的css样式给覆盖了

> dev里的style没有被覆盖。dev里是通过js append
style标签来加入你的element的样式，而build之后你的element基础样式是通过link的css引入的。
https://segmentfault.com/q/1010000014605404

所以 在开发时 我们需要增加自己写的代码的优先级 也就是再在外层包一层css

该bug的element版本为
> "element-ui": "^2.5.4",
