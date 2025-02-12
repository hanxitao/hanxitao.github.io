---
title: splice在for循环中的问题
author: hanxitao
date: 2022-04-27 11:15:00 +0800
categories: [javascript, EverydayLearnOfJs]
tags: [javascript, EverydayLearn]
---

### for循环中使用splice的问题

由于splice会改变原数组，如果在for循环中使用splice就会导致不能够遍历所有元素。

```javascript
const arr = ['a', 'a', 'a', 'b', 'c', 'd', 'a', 'a'];
for (let i = 0; i < arr.length; i ++) {
    if (arr[i] === 'a') {
        arr.splice(i, 1);
    }
}

console.log(arr); // ['a', 'b', 'c', 'd', 'a']
```

### 方法一：改变循环变量

```javascript
const arr = ['a', 'a', 'a', 'b', 'c', 'd', 'a', 'a'];
for (let i = 0; i < arr.length; i ++) {
    if (arr[i] === 'a') {
        arr.splice(i, 1);
        i --;
    }
}

console.log(arr); // ['b', 'c', 'd']
```

### 方案二：倒序遍历

```javascript
const arr = ['a', 'a', 'a', 'b', 'c', 'd', 'a', 'a'];
for (let i = arr.length - 1; i >= 0; i --) {
    if (arr[i] === 'a') {
        arr.splice(i, 1);
    }
}

console.log(arr); // ['b', 'c', 'd']
```