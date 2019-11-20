---
title: 手写一个new
date: 2019-08-16 15:19:05
tags:
---

在写得时候 我们需要首先梳理清楚new这个关键字做了什么   
1.生成一个新的空对象   
2.将这个对象得__proto__指向构造函数得protoType  
3.将构造函数内部this都指向新生成得这个对象  
4.返回 如果构造函数返回的是一个对象那么就返回构造器返回的对象 如果不是就把新创建得对象给返回

```
function _new (){
	let obj = Object.create();
	let constructor = Array.prototype.shift.call(arguments);
	obj.__proto__ = constructor.protoType;
	let res = constructor.apply(obj,arguments);
	return typeof res == 'object' ? res : obj;
}
```
注意原型链的关系


