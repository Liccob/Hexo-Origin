---
title: react中setState中后的"异步"
date: 2019-08-20 19:41:31
tags: 
- setState
categories: 
- React
---

## 现象
在React中，如果是由React引发的合成事件处理（比如通过onClick引发的事件处理），调用setState不会同步更新this.state，而是批量同步更新，造成看起来是异步得样子。除此之外的setState调用会同步执行this.state。所谓“除此之外”，指的是绕过React通过addEventListener直接添加的事件处理函数，还有通过setTimeout/setInterval产生的异步调用，或者是ajax回调中。

## 原因
### 直接原因
在React的setState函数实现中，会根据一个变量isBatchingUpdates判断是直接更新this.state还是放到队列中回头再说，而isBatchingUpdates默认是false，也就表示setState会同步更新this.state，但是，有一个函数batchedUpdates，这个函数会把isBatchingUpdates修改为true，而当React在调用事件处理函数之前就会调用这个batchedUpdates，造成的后果，就是由React控制的事件处理过程setState不会同步更新this.state。 

### 底层原因

最根本得原因肯定是为了性能优化，批量更新渲染肯定比多次渲染更节省性能。在新的concurrent mode中已经支持所有地方产生得更新统一批量处理了。