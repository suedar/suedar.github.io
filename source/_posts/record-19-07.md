---
title: record-19-07
date: 2019-07-24 18:47:21
tags:
---


### void

1. 箭头函数的void用法

> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/void

Non-leaking Arrow FunctionsSection

Arrow functions introduce a short-hand braceless syntax that returns an expression. This can cause unintended side effects by returning the result of a function call that previously returned nothing. To be safe, when the return value of a function is not intended to be used, it can be passed to the void operator to ensure that (for example) changing APIs do not cause arrow functions' behaviors to change.

`button.onclick = () => void doSomething();`

This ensures the return value of doSomething changing from undefined to true will not change the behavior of this code.

上面所属 即箭头函数不想取其返回值的时候 可用void

2. void 可代指未被操作的原始值
某种程度上来说 `void 0 === undefined`