---
title: nuxt.js整合emelentui(自用)
date: 2020-12-31 15:42:32
tags: 
  - nuxt.js
  - elementui
categories:
  - 前端
---


- 创建element-ui.js
- 修改nuxt.config.js配置
其实脚手架已经帮我们搞好了，这篇文章就是基于手动整合为目的的
首先先创建项目，之后，在plugins在创建element-ui.js

```
import Vue from 'vue'
import ElementUI from 'element-ui'

Vue.use(ElementUI);
```
,在nuxt.config.js里面
```
  plugins: [
    '@/plugins/element-ui'   
]

  css: [
     // 1. elementui各组件样式
    'element-ui/lib/theme-chalk/index.css',  
    'element-ui/lib/theme-chalk/display.css'
  ],
```
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281642743.png)
