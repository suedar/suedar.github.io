---
title: js基础之各种基本类型及引用类型的转换之开篇
date: 2017-11-29 20:35:22
tags:
---

  总结一下前端基础的东西，估计会开一个专题，讲的是布尔等基本类型和引用类型等的相互转换问题。

  基础不能丢啊，建议看了这个专题的读者也去总结一些前端的基础，好记性不如烂笔头，代码会更新在[GitHub](https://github.com/suedar/js-)。

 文章大部分内容来自于《JavaScript权威指南》《JavaScript高级程序设计》再加上我最近的总结和理解。

那么我们开始吧。

  提起伟大的JavaScript，就不得不提起他的数据类型，JavaScript的数据类型分为两类：<font color='red'>原始类型（primitive type)</font> 和 <font color='red'>对象类型（object type）</font>。
  而对象类型 又可以叫做引用类型。
所有引用类型都是Object 的实例。

> 碎碎念：伟大的JavaScript由ECMAscript，BOM和DOM组成，而ECMAscript为核心中的核心，BOM（browse object model）为浏览器对象模型，DOM（document object model）为文档对象模型，因为是核心，所以有时我们也称JavaScript数据类型为ECMAscript数据类型。


----------
原始类型有六种：Undefined，Null，Boolean，Number ，String 和 Symbol。
而Symbol是es6新引入的基本数据类型，表示独一无二的值。

关于Symbol具体看看阮大大吧

> http://es6.ruanyifeng.com/#docs/symbol

本专题还没囊括symbol。。待我日后补充。。

剩余的都是对象类型。

引用类型的值是保存在内存中的对象。


----------
造了有这么些基本类型，那咱得有工具把他们几个区分出来啊

于是ECMAscript给了我们typeof工具。


![图图图](http://img.blog.csdn.net/20171129223541311?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

（本图并不是直接连接，只是隐性关系）

由该图可知 typeof 没有null 没有null 没有null

这是头条17年前端面试的一道题哦

另外    `typeof(null) = object`

因为null被认为是一个空对象的引用。

Safari 5及以前版本、chrome 7 及之前版本对正则表达式调用typeof操作符会返回function，而其他浏览器会返回object。

关于浏览器兼容问题 可以访问 [can i  use](https://caniuse.com/)


----------

另外string，数字、布尔值、null和underfined属于不可变类型，不能改变他们。
es6的常量 const 也是如此。

``` js
var a = "你今天很好看";
a.length = 2;
```
![不容置喙](http://img.blog.csdn.net/20171129225519184?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

a.length正常会改变字符串的长度，也就是会把结果变为 你今 ，但是 在这却失效了，这是为什么呢。

a是一个字符串，字符串当然是基本类型值，而下一步的调用却成功了。我们知道，基本类型值不是对象，因为从逻辑上来说他们不该有方法。因此正常来讲a.length会报错呀会报错呀会报错呀

其实，为了让我们实现这种直观的操作，后台已经自动完成了一系列的处理，当第二行代码执行到a.length时，访问过程处于一种读取模式，也就是要从内存读取这个字符串的值，而在读取模式中访问字符串时，后台就会进行如下处理

> （1）创建string类型的一个实例； 
> （2）在实例上调用该方法； 
> （3）销毁这个实例

也就是说

``` js
var a = new String("你今天真好看");
a.length = 2;
a = null;
```

这是在执行a.length的一瞬间后台进行的操作。


![这里写图片描述](http://img.blog.csdn.net/20171129231649391?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

 Boolean Number 和String为ECMAscript的基本包装类型。

> 引用类型和基本包装类型的主要区别在于对象的生存期，使用new操作符创造的引用类型的实例，在执行流离开当前作用域之前一直保存在内存中。而自动创建的基本包装类型的对象，则只存在于一行代码的执行瞬间，然后立即被销毁。这意味着我们不能在运行时为基本数据类型添加属性和方法。

我们可以显示调用Boolean，Number和String来创建基本包装类型的对象。不过，应该在必要的情况下再这么做，因为这种做法很容易让人分不清自己实在处理基本类型还是引用类型的值。对基本包装类型的实例调用typeof会返回object，而且所以基本包装类型的对象在转化为布尔值时的值都是true。

``` js
var value = "25";
var number = Number(value); 
console.log(typeof(number)); //"number"

var obj = new Number(value);
console.log(typeof(obj));  //"object"
```

好啦 开篇就说到这里，做个好梦~

原文地址：https://suedar.github.io
转载请注明出处哦~


----------
更多：
[js基础之关于Boolean及相等运算符的隐式类型转换](http://blog.csdn.net/suedar1/article/details/78685806)