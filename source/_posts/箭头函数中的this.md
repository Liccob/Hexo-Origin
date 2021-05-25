---
title: 箭头函数中的this
date: 2019-10-18 19:49:05
tags: 
- this
categories: 
- JS基础
---

箭头函数中的this一般会保持为在声明这个箭头函数时的this  

函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象。 也就是说 这个this是不能改变的 （例如被apply call bind等函数改变） 

在对象中的多层嵌套时 箭头函数仍旧是保持在最最最外层的this

箭头函数没有自己的this 那么也就不能使用new来调用 因为new最终会返回一个this指向实例，没有this也就没法正常调用，并且箭头函数没有prototype，没有原型链 



