---
title: css之左右摇摆效果
author: hanxitao
date: 2022-04-21 15:42:00 +0800
categories: [css]
tags: [css]
---

## 一、效果如下所示：
![左右摇摆效果](/assets/img/css/swing.gif)

## 二、代码实现

```html
<html>
    <head>
        <style>
            body {
                display: flex;
                justify-content: center;
                align-items: center;
                height: 100vh;
                background: linear-gradient(to top, #537895, #09203f);
            }

            .effect {
                position: relative;
                width: 320px;
                height: 320px;
                border-radius: 50%;
                background-color: white;
                /* 设置对比度 */
                filter: contrast(10);
            }

            .bigball, .smallball {
                position: absolute;
                top: 50%;
                left: 50%;
                transform: translate(-50%, -50%);
                padding: 10px;
                border-radius: 50%;
                /* 设置模糊度，配合上面的constrast来显示圆球的黏性效果 */
                filter: blur(5px);
            }

            .bigball {
                width: 100px;
                height: 100px;
                background: black;
            }

            .smallball {
                width: 60px;
                height: 60px;
                background: red;

                /* 动画：名称 时长 infinite是无限次播放 */
                animation: ball 1s infinite;
            }

            /* 定义小球的动画 */
            @keyframes ball {
                0%, 100% {
                    left: 50px;
                    width: 60px;
                    height: 60px;
                }

                4%, 54% {
                    width: 60px;
                    height: 60px;
                }

                10%, 60% {
                    width: 50px;
                    height: 70px;
                }

                20%, 70% {
                    width: 60px;
                    height: 60px;
                }

                34%, 90% {
                    width: 70px;
                    height: 50px;
                }

                41% {
                    width: 60px;
                    height: 60px;
                }

                50% {
                    left: 270px;
                    width: 60px;
                    height: 60px;
                }
            }
        </style>
    </head>
    <body>
        <div class="effect">
            <div class="bigball"></div>
            <div class="smallball"></div>
        </div>
    </body>
</html>
```