---
title: 模拟实现一个Symbol
date: 2019-08-29 21:03:05
tags: 
- Symbol
categories: 
- JS基础
---

昨天被问到一个问题，手动实现一个Symbol满足
```javascript
const t = symbol('t');
const t2 = symbol('t');
t == t2 //false
const t3 = symbol.for('t');
const t4 = symbol.for('t');
t3 == t4 //true
```

仔细想了想，发现完全无从下手，就上网找了教程看了一遍。  

网上只搜到了如何实现一个symbol的文章[ES6 系列之模拟实现 Symbol 类型](https://github.com/mqyqingfeng/Blog/issues/87)   

整体思路就是先了解具体其特性，如何调用等。之后再一一实现。虽然说起来简单，但是每一步的实现 还是需要仔细思考一下的。  

Symbol ( [ description ] )

When Symbol is called with optional argument description, the following steps are taken:

1.If NewTarget is not undefined, throw a TypeError exception.  
2.If description is undefined, var descString be undefined.
Else, var descString be ToString(description).  
3.ReturnIfAbrupt(descString).  
4.Return a new unique Symbol value whose [[Description]] value is descString.

对于我来说，首先如何声明出这个Symbol函数就是一个问题，由于其是声明到全局的一个方法，并且有可能会遇到需要存储一些变量的情况，那么我们可以使用立即执行函数来隔离作用域并且绑定至全局  

```javascript
(function(){
	const root = this;
	const SymbolPolyfill = function Symbol(description){

	}
	root.SymbolPolyfill = SymbolPolyfill;
})()
```
至此实现一个在全局绑定一个SymbolPolyfill的功能   
下面:如果NewTarget不是undefined 那么就报错  就是不能使用new来声明  
问题就到了new这个运算符 做了什么 我们需要在SymbolPolyfill中做什么样的处理才能使其抛出异常呢？ 或者说 直接调用这个函数 和 用new调用这个函数 有啥区别之处，我们在区别的地方做一层校验抛出异常即可  

从之前的一篇文章，我们可以知道new运算符在使用时实际构造函数中的this是new出来的实例 而直接调用这个构造函数时 this是window 所以我们首先可以添加一行代码 来判断是否是new调用的
```javascript
(function(){
	const root = this;
	const SymbolPolyfill = function Symbol(description){
        if (this instanceof SymbolPolyfill) throw new TypeError('Symbol is not a constructor');
	}
	root.SymbolPolyfill = SymbolPolyfill;
})()
```
如果传进来的desc不是字符串那么就调用其tostring方法 将值赋给description

```javascript
(function(){
	const root = this;
	const SymbolPolyfill = function Symbol(description){
        if (this instanceof SymbolPolyfill) throw new TypeError('Symbol is not a constructor');
		let descString = description === undefined ? undefined : String(description)
	}
	root.SymbolPolyfill = SymbolPolyfill;
})()
```
为了实现相同desc不相等 我们可以返回一个对象 两个对象的存储地址肯定是不相等的

```javascript
(function(){
	const root = this;
	const SymbolPolyfill = function Symbol(description){
        if (this instanceof SymbolPolyfill) throw new TypeError('Symbol is not a constructor');
		let descString = description === undefined ? undefined : String(description)
		var symbol = Object.create(null);
		Object.defineProperty(symbol,{
			'__Description__': {
                value: descString,
                writable: false,
                enumerable: false,
                configurable: false
            }
		})
		return symbol;
	}
	root.SymbolPolyfill = SymbolPolyfill;
})()
```

接下来我们实现for的功能，for就需要用到一个对象存储已经声明过的数据

```javascript
(function(){
	const root = this;
	const SymbolPolyfill = function Symbol(description){
        if (this instanceof SymbolPolyfill) throw new TypeError('Symbol is not a constructor');
		let descString = description === undefined ? undefined : String(description)
		var symbol = Object.create(null);
		Object.defineProperties(symbol,{
			'__Description__': {
                value: descString,
                writable: false,
                enumerable: false,
                configurable: false
            }
		})
		return symbol;
	}
	const forMap = {}
	Object.defineProperties(SymbolPolyfill,{
		'for':{
			value: function(description) {
	            var descString = description === undefined ? undefined : String(description);
	            return forMap[descString] ? forMap[descString] : forMap[descString] = SymbolPolyfill(descString);
	        },
            writable: true,
            enumerable: false,
            configurable: true
		}
	})
	root.SymbolPolyfill = SymbolPolyfill;
})()
```
定义一个属性 这个属性的value是一个函数 这个函数的作用是为了将每一个用symbol.for生成的值都存在一个对象中 如果有相同的desc那么就将已经声明过的值取出来用 这也就完成了 最开始那道面试题的效果