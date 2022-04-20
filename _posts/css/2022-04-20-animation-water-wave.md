---
title: css之水球加载效果
author: hanxitao
date: 2022-04-20 11:04:00 +0800
categories: [css]
tags: [css]
---

## 一、效果如下所示：
![水球加载效果](/assets/img/css/water_wave.gif)

## 二、代码实现

```html
<html>
    <head>
        <style>
            body {
                height: 100vh;
                background: linear-gradient(to bottom, #89f7fe, #66f6ff);
            }

            .wave {
                width: 200px;
                height: 200px;
                background: #2797e7;
                border-radius: 50%;
                position: absolute;
                left: 50%;
                top: 50%;
                transform: translate(-50%, -50%);
                box-shadow: 0 0 50px rgba(255, 255, 255, 0.2);
                overflow: hidden;
            }

            .wave::before {
                content: '';
                width: 300px;
                height: 300px;
                background: rgba(255, 255, 255, 0.8);
                position: absolute;
                left: 50%;
                top: 0;
                transform: translate(-50%, -65%);
                border-radius: 40%;
                animation: wave 3s linear infinite;
            }

            .wave::after {
                content: '加载中';
                position: absolute;
                left: 50%;
                transform: translate(-50%, 40px);
                color: #2797e7;
            }

            @keyframes wave {
                100% {
                    transform: translate(-50%, -65%) rotate(360deg);
                }
            }
        </style>
    </head>
    <body>
        <div class="wave"></div>
    </body>
</html>
```