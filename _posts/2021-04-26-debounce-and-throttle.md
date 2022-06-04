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

```javascript
function throttle(func, delay = 500) {
    let timer = null,
        firstTime = true;

    return function (...args) {
        if (firstTime) {
            // 第一次执行
            func.apply(this, args);
            return firstTime = false;
        }

        if (timer) {
            // 定时器正在执行中，跳过
            return;
        }

        timer = setTimeout(() => {
            clearTimeout(timer);
            timer = null;
            func.apply(this, args);
        }, delay);
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