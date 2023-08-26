---
title: webpack5 基础
date: 2023-05-23 14:01:19
tags:
  - webpack
  - 工具
categories:
  - 前端
---

webpack 的核心价值就是前端源码的打包，即将前端源码中每一个文件（无论任何类型）都当做一个 pack ，然后分析依赖，将其最终打包出线上运行的代码。webpack 的四个核心部分

- entry 规定入口文件，一个或者多个
- output 规定输出文件的位置
- loader 各个类型的转换工具
- plugin 打包过程中各种自定义功能的插件

# 基础配置

## 初始化环境

新建一个文件夹，然后 npm init -y 初始化 npm 环境
安装 webpack ; npm i webpack webpack-cli -D
新建 src 目录并在其中新建 index.js ，随便写点 console.log('sj') 。然后根目录创建 webpack.config.js ，内容如下

```js
const path = require("path");
module.exports = {
  mode: "development",
  entry: path.join(__dirname, "src", "index"),
  output: {
    path: path.join(__dirname, "dist"),
  },
};
```

增加 package.json 的 scripts

```js
"build": "webpack"
```

运行 npm run build ，就可以打包到 dist 目录里面
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202306031616747.png)

## 解析 html

在 src 下面新建 index.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h2>111</h2>
  </body>
</html>
```

npm i html-webpack-plugin -D
npm i webpack-dev-server -D
配置插件

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
  mode: "development",
  entry: path.join(__dirname, "src", "index"),
  output: {
    path: path.join(__dirname, "dist"),
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "src", "index.html"),
      filename: "index.html",
    }),
  ],
  devServer: {
    open: true,
    static: path.join(__dirname, "dist"), // 根目录
  },
};
```

修改 package.json

```json
{
  "name": "webpack-study",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev": "webpack-dev-server"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "html-webpack-plugin": "^5.5.1",
    "webpack": "^5.85.0",
    "webpack-cli": "^5.1.1",
    "webpack-dev-server": "^4.15.0",
    "webpack-merge": "^5.9.0"
  }
}
```

npm run dev 运行
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202306031642892.png)

# 处理 ES6

## 使用 babel

由于现在浏览器还不能保证完全支持 ES6 ，将 ES6 编译为 ES5 ，需要借助 babel 这个神器。安装 babel
npm i babel-loader @babel/core @babel/preset-env -D ，然后修改 配置

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
  mode: "development",
  entry: path.join(__dirname, "src", "index"),
  output: {
    path: path.join(__dirname, "dist"),
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "src", "index.html"),
      filename: "index.html",
    }),
  ],
  devServer: {
    open: true,
    static: path.join(__dirname, "dist"), // 根目录
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: "babel-loader",
        include: path.join(__dirname, "src"),
        exclude: /node_modules/,
      },
    ],
  },
};
```

还要根目录下新建一个 .babelrc.json 文件，内容下

```json
{
  "presets": ["@babel/preset-env"],
  "plugins": []
}
```

修改 index.js 里面的代码

```js
const sum = (a, b) => {
  return a + b;
};

const res = sum(19, 12);
```

npm run dev 运行即可
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202306031654733.png)

### prod

```js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
module.exports = {
  mode: "development",
  entry: path.join(__dirname, "src", "index"),
  output: {
    filename: "bundle[contenthash].js", // 生成的文件名
    path: path.join(__dirname, "dist"),
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: path.join(__dirname, "src", "index.html"),
      filename: "index.html",
    }),
  ],
  module: {
    rules: [
      {
        test: /\.js$/,
        loader: "babel-loader",
        include: path.join(__dirname, "src"),
        exclude: /node_modules/,
      },
    ],
  },
};
```

```json
// build
{
  "name": "webpack-study",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "webpack -config webpack.prod.js",
    "dev": "webpack-dev-server"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "html-webpack-plugin": "^5.5.1",
    "webpack": "^5.85.0",
    "webpack-cli": "^5.1.1",
    "webpack-dev-server": "^4.15.0",
    "webpack-merge": "^5.9.0"
  }
}
```

npm run build
