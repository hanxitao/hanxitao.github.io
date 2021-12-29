---
title: 正则表达式
author: hanxitao
date: 2021-04-23 04:30:00 +0800
categories: [javascript]
tags: [js相关]
---

## 什么是正则表达式
> 正则表达式描述了一种字符串匹配的模式，可以用来检查一个字符串是否含有某种子串、将匹配的子串做替换或者从某个子串中取出符合某个条件的子串等。

## 正则表达式的创建方式
- 字面量
- 实例
```javascript
var reg = /pattern/flags;
var reg = new RegExp(pattern, flags);
```
### 字面量和实例的区别
1. 字面量方式不能进行字符串拼接，实例方式可以
```javascript
var str = 'cm';
var reg = /str/; // /str/
var reg1 = new RegExp(str + 1); // /cm1/
```
2. 字面量方式特殊含义的字符不需要转义，实例方式需要转义
```javascript
var reg = /\d/; // /\d/
var reg1 = new RegExp('\d'); // /d/
```

## 标识符和元字符
### 标识符
- i: 忽略大小写匹配
- m: 多行匹配，即在到达一行文本末尾时还会继续寻找下一行中是否有与正则匹配的项
- g: 全局匹配，模式用于所有字符串，而非在找到第一个匹配项时就停止
### 元字符
1. 表示特殊含义的元字符
> - \d: 0-9之间的任意一个数字
> - \D: 除了\d
> - \w: 数字、字母、下划线
> - \W: 除了\w
> - \s: 空格或者空白
> - \S: 除了\s
> - \b: 匹配单词边界
> - \B: 匹配非单词边界
> - \n：匹配换行符
> - .: 匹配换行符\n之外的任何单字符
> - \\: 转义字符
> - \|: 或者
> - (): 分组
> - \[a-z\]: 表示任意一个
> - ^: 限定开始位置（本身不占位）,[^a-z]表示除了
> - $: 限定结束位置（本身不占位）
2. 表示次数的量词
> - *: 0至多个
> - +: 1至多个
> - ?: 0次或1次
> - {n}: 正好n次
> - {n,}: n至多次
> - {n,m}: n次至m次
3. 注意：()和[]
  - []中出现量词或者\.是没有特殊含义的
  - ()能够提高优先级;不想捕获分组时，可以在()中添加?:来取消分组的捕获
  ```javascript
  var str = "aaabbb";
  var reg = /(a+)(b+)/;
  var reg1 = /(a+)(?:b+)/;
  reg.exec(str); //["aaabbb", "aaa", "bbb"]
  reg1.exec(st1); //["aaabbb", "aaa"]
  ```

## 正则的特性
- 贪婪性

每一次会尽可能多的去捕获符合条件的内容。如果想尽可能少的去捕获符合条件的字符串，可以在量词符号后加?

- 懒惰性

在成功捕获一次后，不管后边的字符串还有没有符合条件的都不再捕获。如果想捕获目标中所有符合条件的字符串的话，使用标识符g来标明是全局捕获
```javascript
var str = "123aaa234";
var reg = /\d+/;
str.match(reg); // [123]
var reg1 = /\d+?/
str.match(reg1); // [1]
```

## 正则相关方法
1. 正则方法
  - reg.test(str)用来验证字符串是否符合正则，符合返回true
  - reg.exec(str)用来捕获符合规则的字符串
  ```javascript
  var str = 'abc123cba456aaa789';
  var reg = /\d+/;
  reg.exec(str); // ["123", index: 3, input: "abc...789"]
  ```
  注意：正则没有加'g'标识符，则多次执行exec捕获的都是同一个；当全局匹配时，多次执行会依次输出下一个匹配到的字符串。
2. 字符串方法
  - str.match(reg)如果匹配成功，就返回匹配成功的数组；如果匹配不成功，则返回null
  ```javascript
  var str = 'abc123cba456aaa789';
  var reg = /\d+/g;
  str.match(reg); // [123, 456, 789]
  ```
  - str.replace(reg)替换匹配到的字符串

    replace方法的第二个参数可以接收一个函数，函数的第一个参数为匹配到的字符串，后面的参数分别对应相应的分组
    ```javascript
    (function (pro) {
        function queryString() {
            var obj = {},
                reg = /([^?&#+]+)=([^?&#+]+)/g;
            this.replace(reg, ($0, $1, $2) => {
                obj[$1] = $2;
            });
            return obj;
        }
        pro.queryString = queryString;
    })(String.prototype);
    ```

## 零宽断言
> 用于查找某些内容（但并不包括这些内容）之前或之后的东西，如\b,^,$那样用于指定一个位置，这个位置应该满足一定的条件。

- 零宽度正预测先行断言（?=exp）：字符串出现的位置的右边必须匹配到exp这个表达式
```javascript
var str = "i'm singing and dancing";
var reg = /\b(\w+(?=ing\b))/g;
str.match(reg); // ["sing", "danc"]
```
- 零宽度负预测先行断言（?!exp）：字符串出现的位置的右边不能是exp这个表达式
```javascript
var str = 'nodejs';
var reg = /node(?!js)/;
reg.test(str); // false
```
- 零宽度正回顾后发断言（?<=exp）：字符串出现的位置的左边必须是exp这个表达式
```javascript
var str = '￥998$888';
var reg = /(?<=$)\d+/;
str.match(reg); // [888]
```
- 零宽度负回顾后发断言（?<!exp）：字符串出现的位置的左边不能是exp这个表达式
```javascript
var str = '￥998$888';
var reg = /(?<!$)\d+/;
str.match(reg); // [998]
```

## 例题
1、把字符串类型的数字每三个分为一组，如str="10000000"，转化为"10,000,000";
```javascript
var reg = /(?=(\B)(\d{3})+$)/g;
var str = "1000000";
str.replace(reg, ".");
```