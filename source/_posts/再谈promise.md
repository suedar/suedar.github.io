---
title: 再谈promise
date: 2019-08-24 17:18:40
tags:
---

两个周末都搭在promise上面啦，所以总结一下吧。

promise1
promise2
error
promise1
promise2
success
error


setTimeout(() => {
console.log('once')
resolve('success')
}, 1000)

a = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('once')
    resolve('success')
  }, 1000)
  console.log(555)
})
console.log(12333)
v = Date.now()
a.then((res) => {
  console.log(res, Date.now() - v)
})
a.then((res) => {
  console.log(res, Date.now() - v)
})

setTimeout(() => {
    console.log(9999)
})

new Promise((resolve, reject) => {
    resolve(3)
}).then(a => {
    console.log(a)
})




setTimeout(() => {
    console.log(1)
})

new Promise((resolve, reject) => {
    console.log(2);
    resolve(3)
}).then(res => {
    console.log(res)
})

console.log(4)