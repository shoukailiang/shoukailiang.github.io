---
layout: create-react-app
title: 'create-react-app 配置scss,ant-design，装饰器，代理，node支持最新语法'
date: 2018-03-28 19:37:07
tags: 
  - 前端
  - js
  - ant-design
  - scss
  - create-react-app
  - webpack
categories:
 - webpack配置 
---
---
## 新建一个项目
```
npm install -g create-react-app
create-react-app my-app
cd my-app
npm i
npm start
# 或者，npm 5.1版本以上自带npx,以下官方推荐
npx create-react-app my-app
cd my-app
npm start
```
## 暴露配置文件
```
# 暴露配置文件，输入yes就好
npm eject
```
## 下载依赖
```
# scss依赖
npm install sass-loader node-sass --save-dev
# 如果node-sass下载不下来的话，即node-sass安装失败使用：
 npm install --save node-sass --registry=https://registry.npm.taobao.org --disturl=https://npm.taobao.org/dist --sass-binary-site=http://npm.taobao.org/mirrors/node-sass
# 说明
--registry=https://registry.npm.taobao.org 淘宝npm包镜像
--disturl=https://npm.taobao.org/dist 淘宝node源码镜像，一些二进制包编译时用
--sass-binary-site=http://npm.taobao.org/mirrors/node-sass 这个才是node-sass镜像
# ant-design
npm install antd --save
# 按需加载的依赖
npm install babel-plugin-import --save
```
## 配置webpack参数
## scss
在config 里面的webpack.config.dev.js和webpack.config.prod.js里面，前面一个是开发的配置文件，后面的是生产时的配置文件
```
大约160行左右
# 第一处是：  
test: /\.css$/ 变成 test: /\.s?css$/  
# 第二处是： 
{loader: require.resolve('sass-loader')}
```

![](https://user-gold-cdn.xitu.io/2018/3/28/1626c594586e794d?w=829&h=536&f=png&s=53733)
之后你随便新建一个a.scss ，import "路径/a.scss"就可以了
> 缺点就是css代码会是全局的，一个人开发还好，多人的话，css命名冲突就很难受了，css-moudle是一种解决方案，但是我不怎么喜欢，我个人推荐可以用下style-component

[css-moudle阮一峰](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)

[style-component](https://www.styled-components.com/)

## ant-design
```
# 修改babel配置，在package.json里面
# 1.按需加载,让nodejs支持最新的语法，装饰器
# package.json里面不能写注释，记得删掉
 "babel": {
    "presets": [
      "react-app",
      [
        "env",
        {
          "targets": {
            "node": true
          }
        }
      ]
    ],
    // 这边是按需加载ant-design的css
    "plugins": [
      [
        "import",
        {
          "libraryName": "antd",
          "libraryDirectory": "es",
          "style": "css"
        }
      ],
      // 装饰器
      "transform-decorators-legacy"
    ]
  },
  //设置代理,应为前端开启了一个服务器，后端又开启了一个服务器，导致跨域的问题，设置代理能解决这个问题
    "proxy": "http://localhost:8888",
```
之后就ok了。你引入一个ant的组件试试就知道了
# 结果
```
# 导入
import { Button } from 'antd';
import React from 'react'
import "./msgCircle.scss";
class MsgCircle extends React.Component {
  render() {
    return (
      <div className="msg-circle">
        <Button type="primary">Primary</Button>
      </div>
    )
  }
}
export default MsgCircle;
# 有人说为什么不用导入css,应为前面已经配置了按需加载
```
# 注意
> 修改配置文件后要重新npm start一下的
- 若配置装饰器后，发现 `vscode` 有红色波浪线，解决方法
```
# 新建一个tsconfig.json，内容如下
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "allowJs": true
  }
}
```