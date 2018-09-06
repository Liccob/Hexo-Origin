---
title: JS中的With
date: 2018-05-25 11:44:09
tags:
---

JS中的With

===

JS中with会建立一个临时的作用于，此作用域中所有的变量在判断其值时都会先在with的对象上查找该属性若有则引用，若无则一层层向外查找。  

下面列出几段代码来说明with得使用方法
```
var qs = location.search.substring(1);
var hostName = location.hostname;
var url = location.href;

//with 
with (location){
    var qs = search.substring(1);
    var hostName = hostname;
    var url = href;
}


function func() {
    console.time("func");
    var obj = {
        a: [1, 2, 3]
    };
    for (var i = 0; i < 100000; i++) {
        var v = obj.a[0];
    }
    console.timeEnd("func");//0.847ms
}
func();



//with
function funcWith() {
    console.time("funcWith");
    var obj = {
        a: [1, 2, 3]
    };
    with (obj) {
        for (var i = 0; i < 100000; i++) {
            var v = a[0];
        }
    }
    console.timeEnd("funcWith");//88.260ms
}
funcWith();
```

导致性能大幅度下降的原因，还是需要说到js编译过程，在js执行之前，js引擎会先解析代码，在不使用with关键字的时候，js引擎知道a是obj上的一个属性，它就可以静态分析代码来增强标识符的解析，从而优化了代码，因此代码执行的效率就提高了。使用了with关键字后，js引擎无法分辨出a变量是局部变量还是obj的一个属性，因此，js引擎在遇到with关键字后，它就会对这段代码放弃优化，所以执行效率就降低了。



下面列几个使用with会出现理解问题的例子：

```

var o={
  x:10,
  foo:function(){
    with(this){
      var x=20;
      var y=30;
      console.log(y);
    }
  }
}

o.foo(); //30
console.log(o.x); //20
console.log(o.y); //undefined
```

调用o.foo()时 foo中的console 会首先查找o是否有y属性，若无向外找到局部变量y

o.x这时已经是20；

o.y因为不存在这个属性则报undefined