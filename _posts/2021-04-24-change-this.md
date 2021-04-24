---
title: Change this
author: hanxitao
date: 2021-04-24 22:21:00 +0800
categories: [javascript]
tags: [this]
---

## 手动实现call
```javascript
Function.prototype.call_ = function (obj) {
  obj = obj ? Object(obj) : window;
  obj.fn = this;
  let args = [...arguments].slice(1);
  let result = obj.fn(...args);

  delete obj.fn;
  return result;
}
```
## 手动实现apply
```javascript
Function.prototype.apply_ = function (obj, arr) {
  obj = obj ? Object(obj) : window;
  obj.fn = this;
  let result;
  if (!arr) {
    result = obj.fn();
  } else {
    result = obj.fn(...arr);
  }

  delete obj.fn;
  return result;
}
```
## 手动实现bind
  - 作用：改变this指向
  - 特点：
    1. 不会执行函数，而是返回一个绑定了this的新函数bound
    2. 无法通过call/apply修改新函数bound的this
    3. 新函数bound可以使用new运算符构造，在构造过程中，已经确定的this会被忽略，返回的实例会继承构造函数的属性与原型属性，并且能正常接收参数
```javascript
const obj = {
  z: 1
};
function fn(x, y) {
  return x + y + this.z;
}
fn.call(obj, 2, 3); // 6
fn.apply(obj, [2, 3]); // 6
var bound = fn.bind(obj, 2);
bound(3); // 6
```

实现：

```javascript
Function.prototype.bind_ = function (obj) {
  if (typeof this !== "function") {
    throw new Error("Function.prototype.bind - what is trying to be dound is not callable");
  }
  const args = [...arguments].slice(1);
  const fn = this;
  function bound() {
    const params = [...arguments].slice(0);
    return fn.apply(this.constructor === fn ? this : obj, args.concat(params));
  }

  function fn_ () {}
  fn_.prototype = fn.prototype;
  bound.prototype = new fn_();
  return bound;
}
```