---
layout: create-react-app
title: 'create-react-app 配置scss,ant-design，装饰器，代理，node支持最新语法，express es6 后端，链接mongodb'
date: 2018-03-28 19:37:07
tags: 
  - 前端
  - javascript
  - ant-design
  - scss
  - create-react-app
  - webpack
  - express
  - mongodb
  - react
categories:
  - 前端
---
---

## 新建一个项目
```shell
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
```shell
# 暴露配置文件，输入yes就好
npm eject
```
## 下载依赖
```shell
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
```shell
大约160行左右
# 第一处是：  
test: /\.css$/ 变成 test: /\.s?css$/  
# 第二处是： 
{loader: require.resolve('sass-loader')}
```

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281525283.png)
之后你随便新建一个a.scss ，import "路径/a.scss"就可以了
> 缺点就是css代码会是全局的，一个人开发还好，多人的话，css命名冲突就很难受了，css-moudle是一种解决方案，但是我不怎么喜欢，我个人推荐可以用下style-component

[css-moudle阮一峰](http://www.ruanyifeng.com/blog/2016/06/css_modules.html)

[style-component](https://www.styled-components.com/)

## ant-design

```javascript
# 修改babel配置，在package.json里面
# 装饰器的包：npm i babel-plugin-transform-decorators-legacy --D
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
```javascript
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
# 如果我又想用antd的主题怎么办
> 这里就给一个官方的解决方案

[传送门](https://gist.github.com/Kruemelkatze/057f01b8e15216ae707dc7e6c9061ef7)

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281525084.png)
# 注意
> 修改配置文件后要重新npm start一下的
- 若配置装饰器后，发现 `vscode` 有红色波浪线，解决方法
```javascript
# 新建一个tsconfig.json，内容如下
{
  "compilerOptions": {
    "experimentalDecorators": true,
    "allowJs": true
  }
}
```
# 链接mongodb，后端node
> 梦想还是要有的，万一实现了呢
```shell
# 在根目录新建一个server 
cd server
# init后就会生成一个package.json，记录你每次安装的包
npm init 
# 为什么把模块装在server里面，装在外面的package.json不好吗，可以啊，不过我喜欢分开
npm i bluebird express mongoose nodemon --save 
mkdir server.js
```
准备启动后端了，链接mongodb
```javascript
const express = require('express');
const mongoose = require('mongoose')

const app = express();

app.use('/', function (req, res) {
  return res.json('hello world')
})
// mongoose的Promise已经废弃了，这里就用下bluebird
mongoose.Promise = require('bluebird');

try {
  mongoose.connect('mongodb://localhost/test', {
    // 不加参数会报警告
    // useMongoClient: true
  })
} catch (error) {
  console.log(error)
}
mongoose.connection
  .once('open', function () {
    console.log('mongoose connection')
  })
  .on('error', function (error) {
    throw error;
  })

app.listen(8888, () => {
  console.log("服务开启在8888");
})
```
> 前面代理的端口要和后端启动的端口一致的

修改package.json
```javascript
# nodemon 就是你不用每次再去手动node server.js了，他会自动的帮你的（在外层的package.json）
  "scripts": {
    "server": "nodemon server/server.js"
  },
```
# express怎么不是es6的语法?
``` shell
# 那就实现一下
# 用babel-cli 
npm i bebel-cli --save 
修改scripts命令
  "server": "NODE_ENV=test nodemon --exec babel-node server/server.js"
# 不指定babel-node的话，默认是node
# 之后你把里面的require改成import是不会报错的
```
# mongodb 存储配置
- 默认你已经安装好mongodb,配好mongodb的环境变量，不配也没关系，多打几个路径而已
- 在某一盘符下新建一个test(名字随意)，里面新建data,etc,logs三个文件夹
- data是存放数据的，etc是配置文件，logs是日志
- 在etc下新建mongo.conf
```shell
# 内容范例
# 数据库路径 (你自己的路径)
dbpath=/home/skl/Desktop/test/data
# 日志输出文件路径 
logpath=/home/skl/Desktop/test/logs/mongodb.log
# 错误日志采用追加模式，配置这个选项后mongodb的日志文件会追加到现有的日志文件，而不是重新创建一个文件 
logappend=true

# 过滤一些无用的日志 
quiet=false

# 启动日志文件，默认启动 
journal=true

# 端口号，默认是27017 
port=27018
```
- 需要注意的是：linux和window的文件分隔符是不一样的，pwd打一下就知道了
- 在etc文件里面运行 mongod --config mongo.conf （指定配置文件）
> 启动server.js前先链接数据库 ，在etc文件里面运行 mongod --config mongo.conf （指定配置文件）
## 启动
```shell
cd server
npm start
```

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281525730.png)
访问localhost:8888，会出现


![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202211281526050.png)

[传送门](https://github.com/shoukailiang/react-scaffolding)  项目放github上了，可以自己查看
