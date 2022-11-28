---
title: vue前后端分离-设置请求代理
date: 2021-04-20 09:29:46
tags: 
  - vue
categories:
 - 前端
---
# vue.config.js
```js
module.exports = {
  devServer: {
      port: 6001, // 端口号，如果端口号被占用，会自动提升1
      host: "localhost", //主机名
      https: false, //协议
      open: true, //启动服务时自动打开浏览器访问
      proxy: { // 开发环境代理配置
          "/dev-api" :{  // '/dev-api': {
              // 目标服务器地址
              target: "localhost",
              changeOrigin: true, // 开启代理服务器，
              pathRewrite: {
                  '^dev-api' :''
              }
          }
      }
  },

  lintOnSave: false, // 关闭格式检查
  productionSourceMap: false, // 打包时不会生成 .map 文件，加快打包速度 
}
```
# 安装请求库
```js
npm install axios --save
```
## request.js工具类
```js
import axios from 'axios'

const service = axios.create({
  baseURL: "/dev-api", 
  timeout: 10000 // request timeout
})

// 请求拦截器
service.interceptors.request.use(
  config => {
    return config
  },
  error => {
    return Promise.reject(error)
  }
)

// 响应拦截器
service.interceptors.response.use(
  response => { 
    // 正常响应
    const res = response.data
    return res
  },
  error => {
    // 响应异常
    return Promise.reject(error)
  }
)

export default service
```

## api接口
```js
import request from '../utils/request'


// 查询用户名是否被注册
export function getUserByUsername(username) {
    return request({
        url: `/system/api/user/username/${username}`,
        method: 'get'
    })
}
```
