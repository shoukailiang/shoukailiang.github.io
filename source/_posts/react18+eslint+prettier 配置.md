---
title: react18+eslint+prettier 配置
date: 2023-01-02 15:47:32
tags:
  - react
  - eslint
  - prettier
  - craco
  - editorconfig
categories:
  - 前端
---

# 新建项目

```js
create-react-app.cmd react18 --template typescript
```

# 配置别名

安装 craco

```js
npm install @craco/craco -D
```

新建 craco.config.js

```js
const path = require("path");
const resolve = (dir) => path.resolve(__dirname, dir);
module.exports = {
  // 配置别名
  webpack: {
    alias: {
      "@": resolve("src"),
    },
  },
};
```

加入红框中的两段
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021525411.png)
package.json 修改

```json
"start": "craco start",
"build": "craco build",
"test": "craco test",
```

![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021528612.png)
便可以使用别名了

```js
import App from "@/App";
```

# 代码规范设置

## editorconfig

新建.editorconfig

```yml
# https://editorconfig.org
root = true

[*]
charset = utf-8
end_of_line = lf
indent_size = 2
indent_style = space
insert_final_newline = true
max_line_length = 80
trim_trailing_whitespace = true

[*.md]
max_line_length = 0
trim_trailing_whitespace = false
```

安装插件
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021530202.png)

## prettier

安装

```
npm install prettier -D
```

安装 vscode 中的插件
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021532032.png)
新建.prettierrc

```yml
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": true,
  "trailingComma": "none",
  "semi": false,
}
```

新建.prettierignore，排除不需要检测的文件

```yml
/build/*
.local
.output.js
/node_modules/**

**/*.svg
**/*.sh

/public/*
```

可以使用 npm run prettier ，直接格式化代码
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021535045.png)

设置成 vscode 默认的格式化
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021536503.png)

## eslint

安装

```
npm i eslint -D
```

配置
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021539691.png)
之后变会自动生成一个配置文件

添加下面的两行，保证 module.export 不会报错。然后 eslint 和 prettier 的标准保持一致
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021540471.png)
打开 vsocode 的设置，可以添加这两行
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021542779.png)

安装插件

```
npm install eslint-plugin-prettier eslint-config-prettier -D
```

当不满足标准的时候，就会报错
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202301021543934.png)
