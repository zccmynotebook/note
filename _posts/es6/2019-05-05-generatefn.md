---
title: generate 函数
tags: es6
---

```
function *foo(x){
  let y=x * (yield 'hello')
  return y
}
var f=foo(2)  //1
var m=f.next()  //2  
console.log(m)//m.value='hello'
f.next(3)//result=6 //3

//1 并没有执行函数，而是创建了一个iterator对象，然后赋值给f;
next()调用的结果是返回一个object对象，value值来自当前yield后面的参数；next传入的值可以看做yield执行的结果值；

yield作为一个表达式，可以发送数据响应next调用，next可以发送数据到暂停的yield;
```