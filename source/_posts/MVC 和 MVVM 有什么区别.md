---
title: MVC 和 MVVM 有什么区别
date: 2023-06-01 11:20:26
tags:
  - MVC
  - MVVM
categories:
  - 设计模式
---

MVC 原理

- View 传送指令到 Controller
- Controller 完成业务逻辑后，要求 Model 改变状态
- Model 将新的数据发送到 View，用户得到反馈

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202306011123725.png)

MVVM 直接对标 Vue 即可

- View 即 Vue template
- Model 即 Vue data
- VM 即 Vue 其他核心功能，负责 View 和 Model 通讯

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202306011123755.png)

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202306011124042.png)
