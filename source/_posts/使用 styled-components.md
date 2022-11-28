---
layout: 使用
title: styled-components
date: 2018-02-26 00:14:49
tags: 
  - 前端
  - react 
  - 框架
  - css
  - javascript
categories:
  - 前端
  - 工具
---

styled-components 是一个常用的 css in js 类库。使得css代码也有了作用域。和所有同类型的类库一样，通过 js 赋能解决了原生 css 所不具备的能力，比如变量、循环、函数等。诸如 sass&less 等预处理可以解决部分 css 的局限性，但还是要学习新的语法，而且需要对其编译，其复杂的 webpack 配置也总是让开发者抵触。而 styled-componens 很好的解决了这些问题，很适合 React 技术栈的项目开发。
## 安装和使用
```
npm install styled-components 
```

```javascript
import React from 'react';
import styled from 'styled-components'
class InfoNav extends React.Component {
  render() {
    const H1 = styled.h1`
      background-color: #a1a;
      text-align: center;
      >span{
        color:blue;
      }
    `;
    return (
      <React.Fragment>
        <H1>
          <span>信息完善页面</span>
        </H1>
      </React.Fragment>
    )
  }
}
export default InfoNav;
```
## 代码提示和高亮
ide 貌似没能做到代码高亮和提示
官网有vscode的插件
```
vscode-styled-components
```

## 组件样式继承
```javascript
import React from 'react';
import styled from 'styled-components'
class InfoNav extends React.Component {
  render() {
    const H1 = styled.div`
      background-color: #a1a;
      text-align: center;
      font-size:20px;
    `;
    const SPAN = H1.extend`
     color: white;
   `;
    return (
      <React.Fragment>
        <H1>
          <SPAN>{this.props.name}信息完善页面</SPAN>
        </H1>
      </React.Fragment>
    )
  }
}
export default InfoNav;
```
## 组件内部使用 className
```javascript
<Wrapper>
  <h4>Hello Word</h4>
  <div className="detail"></div>
</Wrapper>
```
对于这种 styled-components 和 className 混用，或者是一些伪类的情况同样是支持的：
```javascript
import styled from 'styled-components';
const Wrapper = styled.div`
  display: block;
  h4 {
    font-size: 14px;
    &:hover {
      color: #fff;
    }
  }
  .detail {
    color: #ccc;
  }
`;
```
当然还可以通过 injectGlobal 的方式将通用的样式注入到全局中：
```javascript
import styled, { injectGlobal } from 'styled-components';
injectGlobal`
  @font-face {
    font-family: 'Operator Mono';
    src: url('../fonts/Operator-Mono.ttf');
  }

  body {
    margin: 0;
  }
`;
```
## CSS 动画支持
styled-components 同样对 css 动画中的 @keyframe 做了很好的支持。
```javascript
import { keyframes } from 'styled-components';
const fadeIn = keyframes`
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
`;

const FadeInButton = styled.button`
  animation: 1s ${fadeIn} ease-out;
`;
```
keyframes 方法会生成一个唯一的 key 作为 keyframes 的名称，保证它的作用域是在单个文件内，非全局的。
## 总结
下面简单总结一下 styled-components 在开发中的表现：

-  提出了 container 和 components 的概念，移除了组件和样式之间的映射关系，符合关注度分离的模式；
- 可以在样式定义中直接引用到 js 变量，共享变量，非常便利；
- 支持组件之间继承，方便代码复用，提升可维护性；
- 兼容现有的 className 方式，升级无痛；
- 在实际应用中我们完全可以将 style 和 jsx 分开维护

> 原文链接：[https://juejin.im/entry/59a57a2b5188252445327ac1](https://juejin.im/entry/59a57a2b5188252445327ac1 "Title")

> 参考 ： https://www.jianshu.com/p/f188f7ea59b3




