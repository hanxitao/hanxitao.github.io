---
title: 原型
author: hanxitao
date: 2021-05-04 17:09:00 +0800
categories: [javascript]
tags: [js相关]
---

## 一、构造函数
### 1.1 构造函数的实例成员与静态成员
**实例成员**：实例成员就是在构造函数内部，通过this添加的成员。实例成员只能通过实例化对象来访问。
**静态成员**：在构造函数本身上添加的成员，只能通过构造函数来访问
```javascript
function Person(name, age) {
  // 实例成员
  this.name = name;
  this.age = age;
}
// 静态成员
Person.dream = 'money';

let p = new Person('layman', 23);
console.log(p, p.dream); // Person {name: "layman", age: 23} undefined
console.log(Person.name, Person.dream); // 'Person' 'money'
```
### 1.2通过构造函数创建对象
#### 1.2.1 通过new来进行实例化
```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}
let p = new Person('hhh', 23);
``` 
#### 1.2.2 new操作符的原理
1. 创建一个新对象
2. 为新对象添加__proto__属性，指向构造函数的原型prototype
3. 将构造函数的作用域赋值给新对象
4. 执行构造函数中的代码
5. 返回新对象

```javascript
function Person(name) {
  this.name = name;
}

function newFunc(name) {
  const o = {};
  o.__proto__ = Person.prototype;
  Person.call(o, name);
  return o;
}
```
#### 1.2.3 判断当前函数是普通调用还是new调用
```javascript
// 方法一
function fun() {
    if (this.constructor === fun) {
        console.log('new 调用');
    } else {
        console.log('普通调用');
    }
}
// 方法二
function fun() {
  // new.target指向被new调用的构造函数
  console.log(new.target === fun);
}
```

### 1.2.4 包装对象
某些场合，原始类型的值会自动当作包装对象调用，即调用包装对象的属性和方法。这时，**javascript引擎会自动将原始类型的值转为包装对象实例，并在使用后立刻销毁实例**。例如字符串s = 'hello world';s.x = 123;可以拆分为如下步骤：
```javascript
var sObj = new Srting(s);
sObj.x = 123;
sObj = null;
sObj.x; // undefined
```
上面代码为字符串s添加了一个x属性，结果总是返回undefined。其实，调用结束后，包装对象实例会自动销毁，这意味着，下一次调用字符串的属性时，实际上是调用一个新生成的对象，而不是上一次调用时生成的那个对象，所以取不到赋值在上一个对象的属性。

## 二、原型和原型链
### 2.1 原型
#### 2.1.1 什么是原型？
每一个函数都有一个prototype属性，指向另一个对象，即原型对象。prototype上的所有属性和方法都会被构造函数的实例继承
#### 2.1.2 原型的作用
可以把公用的属性和方法定义在prototype上
#### 2.1.3 关于原型的方法
- Object.create()方法创建一个新对象，使用现有的对象来提供新创建的对象的__proto__（该方法的参数只能是对象或者null）
```javascript
const person = {
  firstName: 'han',
  sayMyname() {
    console.log(this.firstName + this.lastName);
  }
};
const me = Object.create(person);
me.lastName = 'xitao';
me.sayMyname(); // 'hanxitao'
```
- Object.getPrototypeOf(obj)方法返回指定对象的原型
```javascript
Object.getPrototypeOf(me); // {firstName: "han", sayMyname: ƒ}
```
- Object.setPrototypeOf()方法设置一个指定的对象的原型到另一个对象

#### 2.1.4 判断某条属性是对象自身属性而非原型属性？
- in：判断的是对象的所有属性，包括对象实例及其原型的属性
- hasOwnProperty：判断对象自身属性中是否具有指定的属性

```javascript
Person.prototype.firstName = 'han';
function Person(name) {
  this.lastName = name;
}
const p = new Person('xitao');
console.log('lastName' in p); // true
console.log('firstName' in p); // true

console.log(p.hasOwnProperty('lastName')); // true
console.log(p.hasOwnProperty('firstName')); // false
```
#### 2.1.5 构造函数与原型的关系图
![](/assets/img/favicons/prototype.jpg)
### 2.2 原型链
#### 2.2.1 什么是原型链？
实例对象与原型之间的连接叫做原型链
#### 2.2.2 原型链的作用
由构造函数构造出的实例都有一个内置的属性__proto__，指向构造函数的prototype对象。当访问一个对象的属性时，如果这个对象本身不存在这个属性，就会通过__proto__去构造函数的原型prototype中去找。
```javascript
Person.prototype.eat = function () {
  console.log(this.name + '在吃饭');
}
function Person(name, age) {
  this.name = name;
  this.age = age;
}
const p = new Person('aaa', 11);
p.eat(); // 'aaa在吃饭'
```

#### 2.2.3 instanceof
- 定义：instanceof运算符用于检测构造函数的prototype属性是否会出现在某个实例对象的原型链上
- 语法：object instanceof constructor
```javascript
son instanceof Father // true
son instanceof Son // true
```

#### 2.2.4 原型链示意图
![](/assets/img/favicons/proto.png)

## 三、继承
### 3.1 继承父构造函数的属性和方法
```javascript
function Father(name) {
  this.name = name;
  this.sayHello = function () {
    console.log('hello');
  }
}

function Son(name) {
  Father.call(this, name);
}
```

### 3.2 继承父构造函数原型上面的属性和方法
方法一：Son.prototype = Father.prototype
```javascript
Father.prototype.sayHello = function () {
  console.log('hello');
}
function Father(name) {
  this.name = name;
}

Son.prototype = Father.prototype;
Son.prototype.hobby = 'game'; // 此操作会影响到父类构造函数的原型 
function Son(name, age) {
  Father.call(this, name);
  this.age = age;
}

const son = new Son('aa', 11);
const father = new Father('bb');

console.log(son.hobby); // 'game'
console.log(father.hobby); // 'game'
```

方法二：子构造函数的原型指向父构造函数的实例
```javascript
var inherit = (function () {
    var F = function () {};
    return function (Target, Origin) {
        F.prototype = Origin.prototype;
        Target.prototype = new F();//不能与上边一行交换位置
        Target.prototype.constructor = Target;//让Target构造函数的constructor归位
        Target.prototype.ancestor = Origin.prototype;//使构造出的对象能够找到自己的祖先是谁，也就是真正继承于谁
    }
}());
```