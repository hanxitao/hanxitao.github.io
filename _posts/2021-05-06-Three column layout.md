---
title: Three column layout
author: hanxitao
date: 2021-05-06 16:06:00 +0800
categories: [css]
tags: [css]
---

## 一、流体特性+BFC特性的三栏布局
代码如下：
```html
<html lang="en">
<head>
  <style>
    .common {
      width: 200px;
      height: 30px;
      float: left;
    }
    .left {
      background: green;
    }
    .right {
      float: right;
      background: blue;
    }
    .center {
      height: 50px;
      overflow: hidden;
      background: red;
    }
  </style>
</head>
<body>
  <div class="common left"></div>
  <div class="common right"></div>
  <div class="center"></div>
</body>
</html>
```
效果如下：
![](/assets/img/favicons/layout2.png)
### 1.1 流体特性
#### 1.1.1 什么是流体特性？
**块状水平元素**，如div元素，在默认情况下（非浮动、绝对定位等），**水平方向会自动填满外部的容器**；*如果该元素自身有margin-left/margin-right、padding-left/padding-right、border-left-width/border-right-width等，实际内容区域会变小。*

代码如下：
```html
<html lang="en">
<head>
  <style>
    .wrap {
      width: 500px;
      height: 100px;
      border: 1px solid black;
    }
    .box {
      height: 50px;
      background: peru;
      margin-left: 50px;
    }
  </style>
</head>
<body>
  <div class="wrap">
    <div class="box"></div>
  </div>
</body>
</html>
```
效果如下：

![](/assets/img/favicons/liutitexing1.png)

由图片可以看出，box左侧有使用margin-left留出的50px的空白，且其内容区域会自适应。所以，可以在左侧的空白区域使用浮动元素来实现两栏布局。

#### 1.1.2 使用流体特性实现布局的缺点
当使用流体特性的块状元素与浮动元素做兄弟时，是覆盖的效果，即浮动元素会覆盖流体特性的元素，如下图。为了避免这种情况，就需要知道浮动元素的大小，与中间部分自适应相违和。

![](/assets/img/favicons/layout1.png)