---
title: first-child与first-of-type的区别
author: hanxitao
date: 2021-05-15 16:45:00 +0800
categories: [css]
tags: [css]
---

### :first-child匹配的是父元素的第一个子元素，可以说是**结构上的第一个子元素**

代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <style>
    div p:first-child {
      background: lightblue;
    }
    div h1:first-child {
      background: lightcoral;
    }
    div span:first-child {
      background: lightseagreen;
    }
  </style>
</head>
<body>
  <div>
    <p>第一个子元素p</p>
    <h1>第二个子元素h1</h1>
    <span>第三个子元素span</span>
    <span>第四个子元素span</span>
  </div>
</body>
</html>
```
效果如下图：
![](/assets/img/first-child/first-child1.PNG)
- p:first-child 匹配到的是p元素，因为p元素是div元素的第一个子元素
- h1:first-child 匹配不到任何元素，因为h1是div的第二个子元素，而不是第一个
- span:first-child 匹配不到任何元素，因为这里两个span元素都不是div的第一个子元素

### :first-of-type匹配的是某父元素下**相同类型中的第一个子元素**

把上述代码中的first-child替换为first-of-type:
```css
div p:first-of-type {
  background: lightblue;
}
div h1:first-of-type {
  background: lightcoral;
}
div span:first-of-type {
  background: lightseagreen;
}
```
效果如下图：
![](/assets/img/first-child/first-child2.PNG)