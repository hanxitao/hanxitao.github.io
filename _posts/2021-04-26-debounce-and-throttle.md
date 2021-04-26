---
title: debounce and throttle
author: hanxitao
date: 2021-04-25 15:30:00 +0800
categories: [javascript]
tags: [debounce]
---

## 防抖
&#8195;**所谓防抖，就是指触发事件后n秒后才执行函数，如果在n秒内又触发了事件，则会重新计算函数的执行时间**
```javascript
function debounce(func, delay) {
    let timer;

    return function () {
        clearTimeout(timer);
        timer = setTimeout(() => {
            func(...arguments);
        }, delay);
    }
}
```

## 节流
&#8195;**所谓节流，就是指连续触发事件在n秒中只执行一次函数**

定时器版：开始时不执行，最后时间间隔内再执行一次
```javascript
function throttle(func, delay) {
    let timer;

    return function () {
        if (!timer) {
            timer = setTimeout(() => {
                timer = null;
                func(...arguments);
            }, delay);
        }
    }
}
```

时间戳版：开始时立即执行一次，最后时间间隔内不再执行
```javascript
function throttle1(func, delay) {
    let initTime = 0;

    return function () {
        let now = Date.now();
        if (now - initTime > delay) {
            func(...arguments);
            initTime = now;
        }
    }
}
```
