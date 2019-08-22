---
title: git中submoduule的使用
date: 2019-08-22 20:53:36
tags:
---
最近公司两个项目需要融合 所以大家维护了一个公共库 又不想使用npm每次都发包 所以就用内嵌git库的方式来维护  
内嵌git库主要使用的就是 submodule的方式 就是git库内嵌git库  
首先需要在外层设置你的submodule的一些信息
```
[submodule "test_mp"]
	path = test_mp
	url = https://coding.xx.com/app/test_mp.git
```
每个clone下来外层库的人 首先需要git submodule init进行子库的初始化 之后需要再进行git submodule update来将子库clone至本地代码库 多个子库或者嵌套子库的话加上--recursive  
之后在子库中修改东西时 需要在子库进行提交 然后cd出外层提交外层的代码 如果想在外层更新子库的代码就是git submodule 在子库的文件夹内的话就直接git pull即可  
[参考指令链接](http://easior.is-programmer.com/posts/42541.html) 
