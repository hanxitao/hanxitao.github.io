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

### 1.2 BFC
#### 1.2.1 什么是BFC?
取W3C对[BFC](http://www.ayqy.net/doc/css2-1/visuren.html#block-formatting)的定义：
> 浮动，绝对定位的元素，非块盒的块容器（例如inline-blocks, table-cells和table-captions），以及'overflow'不为'visible'的块盒会为其内容建立新的块格式化上下文。

#### 1.2.2 BFC渲染规则？
1. 在一个块格式化上下文中，盒在垂直方向一个接一个地放置，从包含块的顶部开始。
2. 两个兄弟盒之间的垂直距离由'margin'属性决定。
3. 同一个块格式化上下文中的相邻块级盒之间的垂直外边距会合并。
4. 在一个块格式化上下文中，每个盒的左外边界挨着包含块的左外边界，即使浮动元素也成立
5. BFC的区域不会与浮动元素重叠
6. 计算BFC的高度时，浮动元素也参与计算

#### 1.2.3 BFC规则详解
1. **同一个块格式化上下文中的相邻块级盒之间的垂直外边距会合并**

    这里是讲margin collapse，其实是有如下情况会发生合并：1.父子外边距；2.兄弟外边距；不过需要符合以下几点：
    1. **需要属于普通流中的盒子**：也就是不脱离文档流的
    2. **毗邻**：元素间没有被padding、border、clear和line box分隔开
    3. **垂直**：margin-top和margin-bottom

    代码如下：

    ```html
    <!DOCTYPE html>
    <html lang="en">
    <head>
      <style>
        .parent {
          width: 200px;
          height: 200px;
          background: orchid;
        }
        .child {
          width: 100px;
          height: 100px;
          margin-top: 30px;
          background: palegoldenrod;
        }
      </style>
    </head>
    <body>
      <div class="parent">
        <div class="child"></div>
      </div>
    </body>
    </html>
    ```

    效果如下：
    ![](/assets/img/favicons/bfc1.png)

    该种现象即为margin塌陷：父子嵌套元素在垂直方向上的margin，父元素的margin-top和子元素的margin-top重合取最大值，并且作用在父元素上。
    
    解决：给父元素添加overflow:hidden;即可解决

2. **BFC的区域不会与浮动元素重叠**

    在该种方式实现三栏布局时，在左右浮动的两个元素后面跟了一个具有流体特性的div元素，此时浮动元素会覆盖在这个具有流体特性的div元素上。在给该元素加上overflow:hidden;后，会自动退避浮动元素宽度的距离。


## 二、float实现三栏布局（全部float:left）
代码如下：
```html
<html lang="en">
<head>
  <style>
    .wrap2 {
      overflow: hidden;
      margin-top: 30px;
      padding-left: 200px;
      padding-right: 250px;
      border: 1px solid red;
    }
    .wrap2 div {
      position: relative;
      float: left;
      height: 30px;
    }
    .left2 {
      width: 200px;
      left: -200px;
      margin-left: -100%;
      background: orange;
    }
    .right2 {
      width: 250px;
      left: 250px;
      margin-left: -250px;
      background: orangered;
    }
    .center2 {
      width: 100%;
      background: orchid;
    }
  </style>
</head>
<body>
  <div class="wrap2">
    <div class="center2">center2</div>
    <div class="left2">left2</div>
    <div class="right2">right2</div>
  </div>
</body>
</html>
```
没有设置负margin和left值之前：
![](/assets/img/favicons/layout-float1.png)

左边的盒子设置margin-left:-100%;可以将其拉至中间盒子最左边：
![](/assets/img/favicons/layout-float2.png)

右边的盒子设置margin-left:-250px;是其相对中间盒子向左移动自身的宽度：
![](/assets/img/favicons/layout-float3.png)

最后左右盒子相对自己分别移动自身的宽度即可实现三栏布局：
![](/assets/img/favicons/layout-float4.png)

## 三、使用flex实现三栏布局
代码如下：
```html
<html lang="en">
<head>
  <style>
    .wrap1 {
      display: flex;
      border: 1px solid black;
    }
    .left1, .right1 {
      width: 200px;
      height: 40px;
      background: lemonchiffon;
    }
    .center1 {
      flex: 1;
      height: 45px;
      background: lightcoral;
    }
  </style>
</head>
<body>
  <div class="wrap1">
    <div class="left1">left</div>
    <div class="center1">center</div>
    <div class="right1">right</div>
  </div>
</body>
</html>
```