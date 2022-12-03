---
title: Unity 子线程调用主线程
date: 2021-08-04 10:20:30
tags: 
  - unity
  - 线程
categories:
 - unity
---

在Unity中，子线程是无法调用Unity主线程的API的，因为unity不允许这么干。例如在线程中使用gameObject.setActive(false).

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202212031241251.png)

# 参考
https://zhuanlan.zhihu.com/p/107698641