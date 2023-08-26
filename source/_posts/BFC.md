---
title: BFC
date: 2023-03-05 18:32:06
tags:
  - css
  - BFC
categories:
  - 前端
---

# BFC 的理解

块级格式化上下文，它是指一个独立的**块级渲染区域**，只有 Block-level BOX 参与，该区域拥有套渲染规则来约束块级盒子的布局，且与区域外部无关。

# 从一个现象开始说起

- 一个盒子不设置 height,当内容子元素都浮动时，无法撑起自身
- 这个盒子没有形成 BFC

# 如何创建 BFC

- float 的值不是 none
- position 的值不是 static 或者 relative
- display 的值是 inline-block,flex,或者 inline-flex
- overflow:hidden

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .father {
        /* bfc1 */
        /* float: left; */
        /* bfc2 */
        /* position: absolute; */
        /* bfc3 */
        /* display: inline-block; */
        /* bfc4 */
        overflow: hidden;
      }
      .son {
        width: 200px;
        height: 200px;
        background-color: aliceblue;
        float: left;
      }
    </style>
  </head>
  <body>
    <div class="father">
      <div class="son"></div>
      <div class="son"></div>
      <div class="son"></div>
    </div>
  </body>
</html>
```

# BFC 的其他作用

- BFC 可以取消盒子的 margin 塌陷

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .father {
        width: 200px;
        height: 300px;
        background-color: antiquewhite;
        /* overflow: hidden; */
      }
      .son {
        width: 100px;
        height: 100px;
        background-color: aliceblue;
        margin: 20px;
      }
    </style>
  </head>
  <body>
    <div class="father">
      <div class="son"></div>
    </div>
  </body>
</html>
```

- BFC 可以阻止元素被浮动元素覆盖

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .son {
        width: 100px;
        height: 100px;
        background-color: aliceblue;
        float: left;
      }

      .son-last {
        width: 200px;
        height: 200px;
        background-color: aqua;
        /* 可以阻止元素被浮动元素覆盖 */
        overflow: hidden;
      }
    </style>
  </head>
  <body>
    <div class="father">
      <div class="son"></div>
      <div class="son"></div>
      <div class="son-last"></div>
    </div>
  </body>
</html>
```
