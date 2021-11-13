---
title: 手写call、apply和bind
author: hanxitao
date: 2021-04-24 22:21:00 +0800
categories: [javascript]
tags: [手写]
---

## call、apply和bind的作用

   call、apply和bind共同的作用是：改变this指向

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
    3. 绑定函数bound也能使用new操作符创建对象：这种行为就像把原函数当成构造器，提供的this被忽略。

例子如下：

```javascript
  var value = 2;

  var foo = {
    value: 1
  }

  function bar(name, age) {
    this.habbit = 'shopping';
    console.log(this.value);
    console.log(name);
    console.log(age);
  }
  bar.prototype.friend = 'kevin';

  var bindFoo = bar.bind(foo, 'daisy');
  var obj = new bindFoo(18);
  // undefined
  // daisy
  // 18
  console.log(obj.habbit);//shopping
  console.log(obj.friend);//kevin
```

实现：

```javascript
Function.prototype.bindWithSelf = function (context) {
  if (typeof this !== "function") {
    throw new Error("Function.prototype.bind - what is trying to be dound is not callable");
  }

  const self = this,
    args = Array.prototype.slice.call(arguments, 1),
    f = function () {};

  const boundFunc = function () {
    const boundArgs = Array.prototype.slice.call(arguments);
    self.apply(this instanceof self ? this : context, args.concat(boundArgs));
  }

  f.prototype = this.prototype;
  boundFunc.prototype = new f();

  return boundFunc;
}
```

## 参考文章
来自juejin.com的[JavaScript深入之bind的模拟实现](https://juejin.cn/post/6844903476623835149#comment)