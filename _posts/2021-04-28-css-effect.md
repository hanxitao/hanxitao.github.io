---
title: Css effect
author: hanxitao
date: 2021-04-28 10:50:00 +0800
categories: [css]
tags: [css]
---
## 1、transform中scale
```css
#wrap {
  position: relative;
  padding: 8px;
  background: peru;
  display: inline-block;
}
#wrap::after {
  content: "";
  width: 100%;
  height: 2px;
  background: red;
  position: absolute;
  left: 0;
  bottom: -2px;
  transform: scaleX(0);
  transition: .3s ease-in-out;
}
#wrap:hover::after {
  transform: scaleX(1);
}
```
效果：
![](/assets/img/favicons/scale.gif)

## 2、linear-gradient
```css
.wrap {
  width: 100px;
  height: 2px;
  background-image: linear-gradient(to right, #9382e3 60%, transparent 60%);
  background-size: 10px 2px;
}
```
效果：
![](/assets/img/favicons/linear-gradient.png)