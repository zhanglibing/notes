---
layout: post
title:  "javascript 模块"
tags:
categories:
---

## javascript 易错总结

```javascript
//math.js
//将方法挂载在exports对象上作为属性即可导出
exports.add = function(){
  var sum = 0,
      i = 0,
      args = arguments,
      l = arguments.length;
  while(i<l){
    sum += args[i++];
  }
  return sum;
}
```
