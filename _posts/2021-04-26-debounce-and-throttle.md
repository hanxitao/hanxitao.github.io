---
title: 防抖节流
author: hanxitao
date: 2021-04-25 15:30:00 +0800
categories: [javascript]
tags: [js相关]
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

## 应用场景
- debounce
  - 搜索联想，用户在不断输入值时，用防抖来节约请求资源
  - window触发resize时，不断地调整浏览器窗口大小会不断地触发这个事件，用防抖来让其只触发一次
- throttle
  - 鼠标不断点击触发，mousedown（单位时间内只触发一次）
  - 监听滚动事件，比如是否滑到底部自动加载更多，用throttle来判断