---
title: 0.5px border
date: 2018-05-22 18:38:37
tags:
---
如何设置0.5px的边框
===

前几天在实际业务中需要移动短展示四个0.5px宽度的边框，之前一直不到怎么写，都是自己摸索用scale写的，但是scale后整个content虽然展示出来的是缩小了0.5倍，但是在实际的布局中还是会占用原先的空间。单个的0.5px的边框比较好处理但是，可以直接用伪类before，after写出来，后来就想可不可以整个四个边框都用一个伪类来写出。  
但是这种方式只适用于固定宽高的元素，因为需要用两倍数值来保持宽高呗缩放0.5倍之后的大小



```
<span id="demo">test</span>

#demo::before{
  content: '';
  position: absolute;
  width: 48px;
  height: 28px;
  border-radius: 2px;
  border: 1px solid #999;
  -webkit-transform-origin: 0 0;
  -moz-transform-origin: 0 0;
  -ms-transform-origin: 0 0;
  -o-transform-origin: 0 0;
  transform-origin: 0 0;
  -webkit-transform: scale(0.5, 0.5);
  -ms-transform: scale(0.5, 0.5);
  -o-transform: scale(0.5, 0.5);
  transform:translate(-2px,-1px) scale(0.5, 0.5);
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box; 
  box-sizing: border-box;

}


```

DONE