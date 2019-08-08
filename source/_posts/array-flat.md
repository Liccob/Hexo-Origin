---
title: array.flat()
date: 2019-08-08 14:43:49
tags:
---


### 实现Array.flat()方法  
flat() 方法会按照一个可指定的深度递归遍历数组，并将所有元素与遍历到的子数组中的元素合并为一个新数组返回。
```

Array.prototype.flat= function() {
    return [].concat(...this.map(item => (Array.isArray(item) ? item.flat() : [item])));
}

```

在原型上修改 无限深度递归数组 使其扁平化