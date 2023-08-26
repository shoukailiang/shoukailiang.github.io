---
title: commitlint报错问题
date: 2023-08-26 22:44:32
tags: 
  - git
categories:
 - 前端
---


# commit一直有问题

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202308262213444.png)

后来发现官网
```
 echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js 
```
生成的代码时UTF16的，无语。
改成utf-8就好了