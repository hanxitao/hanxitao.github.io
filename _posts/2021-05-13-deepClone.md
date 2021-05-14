---
title: deepClone
author: hanxitao
date: 2021-05-13 13:25:00 +0800
categories: [javascript]
tags: [deepClone]
---

```javascript
function deepClone(origin, target) {
  var target = target || {},
    toStr = Object.prototype.toString,
    arrStr = '[object Array]';
  
    for (var prop in origin) {
      if (origin.hasOwnProperty(prop)) {
        if (typeof(origin[prop]) !== null && typeof(origin[prop]) == 'object') {
          target[prop] = toStr.call(origin[prop]) == arrStr ? [] : {};
          deepClone(origin[prop], target[prop]);
        } else {
          target[prop] = origin[prop];
        }
      }
    }

    return target;
}
```