---
title: css属性转换成驼峰式
date: 2023-02-24 16:18:32
tags:
  - css
categories:
  - 算法
  - 前端
---

```js
function convertToCamelCase(cssProperty) {
  if (cssProperty[0] == "-") {
    // slice不会修改原数组，但是splice会
    cssProperty = cssProperty.slice(1);
  }
  return cssProperty.replace(/-([a-z])/g, function (match, letter) {
    // 匹配到（）里面的
    return letter.toUpperCase();
  });
}

console.log(convertToCamelCase("font-size")); // 输出：fontSize
console.log(convertToCamelCase("-webkit-text-stroke")); // 输出：webkitTextStroke
```
