---
title: js基础之关于Boolean及相等运算符的隐式类型转换
date: 2017-12-01 20:36:53
tags:
---


## Boolean函数

根据 [w3c规范](http://www.w3school.com.cn/js/js_obj_boolean.asp) 布尔对象共有这几种方法：

| 方法 | 描述|
|---|---|
|<a href="/jsref/jsref_tosource_boolean.asp">toSource()</a>|返回该对象的源代码。|
|<a href="/jsref/jsref_toString_boolean.asp">toString()</a>|把逻辑值转换为字符串，并返回结果。|
|<a href="/jsref/jsref_valueOf_boolean.asp">valueOf()</a>|返回 Boolean 对象的原始值。|

布尔值常用于JavaScript中的控制语句中。例如JavaScript的if/else语句，如果布尔值为true执行第一段逻辑，如果为false执行另一段逻辑。

JavaScript的值都可以转化为布尔值，下面这些值会被转换为false：
``` js
undefined
null
0
-0
NaN
""//空字符串
```
所有其他值，包括所有的对象（数组）都会转化为true。

也就是说：
``` js
var c = Boolean([]);
console.log(c); // true
```

结果为真。


----------
## boolean应用

Boolean常常应用的有两种，一种是if(a==b)?，一种是if(o)？。


----------


#### if（o）？

对于第二种，只要当o不是false，null和undefined时就会执行if之后的代码。
也就是说 `if({}&&[])`，为真。

但是


```
var fn = function() {};

if (fn && fn()) {
    console.log(22222);
}
```

这个条件判断执不执行呢？
结果是不执行的，因为`fn()` 是undefined啊小傻瓜

```
var obj = {};
var string = "";
var array = [];
var fn = function() {};

if (obj) {
    console.log("obj");
}
if (string) {
    console.log("string");
}
if (array) {
    console.log("array"); //空字符串不输出
}
if (fn) {
    console.log("fn");
}
```

结果为

![输出](http://img.blog.csdn.net/20171201122624023?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

还是那句话咯 

<font color="red">undefined、null、0、-0、NaN、"" </font> 为false

![这里写图片描述](http://img.blog.csdn.net/20171201124356493?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![这里写图片描述](http://img.blog.csdn.net/20171201124412043?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

----------

#### if（a==b）？

这种应用呢，需要用到隐式类型转换，比较麻烦一点

== 和 === 运算符用于比较两个值是否相等，但是===为严格相等，它不会进行隐式类型转换，==会进行隐式类型转换。

严格相等运算符”===“首先计算其操作数的值 然后比较这两个值。

![===](http://img.blog.csdn.net/20171201123451620?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


----------


相等运算符”==“

![这里写图片描述](http://img.blog.csdn.net/20171201123701337?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20171201123714027?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

也就是说：

![这里写图片描述](http://img.blog.csdn.net/20171201125106009?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![这里写图片描述](http://img.blog.csdn.net/20171201124930815?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvc3VlZGFyMQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

懵了吗？哈哈 一言以蔽之，这之中所有的比较都是倾向于转化为数值再进行运算的。

``` js
if (null == undefined)  // true
if (null == 0) 
if (null == NaN)
if (null == '') 
if (undefined == 0)
if (undefined == '')
if (undefined == NaN)
if (NaN == 0) 
if (NaN == '')
if (0 == '') //true
if (NaN == false)
if (0 == false)//true
if ('' == false)//true
if (undefined == false)
if (null == false)
if (NaN == true)
if (0 == true)
if ('' == true) 
if (undefined == true)
if (null == true)
if ([] == null)
if ([] == undefined)
if ([] == 0)//true
if ([] == '') //true
if ([] == NaN)
if ([] == false)//true
if ({} == null)
if ({} == undefined)
if ({} == 0) 
if ({} == '') 
if ({} == NaN) 
if ({} == false)
if([1]==[1])
if({a:1}=={a:1})
```

说说最后两个是为嘛吧，因为其都为对象，对象为引用值晓得吧，所以比较的是内存中的位置，指针指向的位置不一样，所以不相等。

这个面试笔试又常常考，所以尽力记住咯。
我把几个对的拎出来了 ↓

``` js
if (null == undefined)  // true
if (0 == '') //true
if (0 == false)//true
if ('' == false)//true
if ([] == 0)//true
if ([] == '') //true
if ([] == false)//true
```

代码更新在github 欢迎验证

> https://github.com/suedar/js-/tree/master/Boolean