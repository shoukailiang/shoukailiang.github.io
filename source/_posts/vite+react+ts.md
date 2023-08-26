---
title: vite+react+ts
date: 2023-08-04 16:44:32
tags: 
  - react
  - vite
  - typescript
categories:
 - 前端
---


# 遇到的一些问题
最近在用vite写react项目，记录一下遇到的问题

> 配置别名后，在vscode中path有波浪线
-  npm install @types/node --save-dev

> 配置完成后，vscode中使用@还是有波浪线，但是项目没报错
```js
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "resolveJsonModule": true,
    "isolatedModules": true,
    "noEmit": true,
    "jsx": "react-jsx",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    // 加这里=================================
    "paths": { 
      "@/*": ["./src/*"], 
    } 
  },
  "include": ["src"],
  "references": [{ "path": "./tsconfig.node.json" }]
}
```

> 神奇的bug
![](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202308051524370.png)

将首字母改成大写就好了。

> 后端返回的res为underfined，但是网络面板里是成功的

后来发现是ts 类型的问题，他as 了一下就没了，主要还是前后端定义的格式有问题
```js
export type ResType = {
  code: number
  data?: DataType
  msg?: string,
  msgs?:any,
  users?:object,
}

export type DataType = {
  [key: string]: any
}
```

> 上一行执行一个reducer，nav(-1)，然后发现reducer不执行
后来我放回调里面了
```js
  const { run: readMsgRun, loading: readMsgLoading } = useRequest(
    async (to) => {
      const data = await readMsgService(to);
      return {num:data,from:to};
    },
    {
      manual: true,
      onSuccess(res: any) {
        console.log(res)
        dispatch(msgReadReducer({ num:res.num, from: res.from }));
        console.log({ num:res, userid: _id })
      },
      onFinally() {
        nav(-1);
      }
    }
```