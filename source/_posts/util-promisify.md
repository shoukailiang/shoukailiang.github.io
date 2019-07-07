---
title: util.promisify
date: 2018-07-21 20:27:01
tags: 
  - 前端
  - node
  - promise 
categories:
  - 前端
  - promise
---
# 原始的写法回调函数
```javascript
const fs =require('fs')
fs.readFile('./package.json',(err,data)=>{
  if(err) return console.log(err)
  data=JSON.parse(data)
  console.log(data.name)
})
```
# 用了promise
```javascript
const fs =require('fs')

function readFileAsync(path){
  return new Promise((resolve,reject)=>{
    fs.readFile(path,(err,data)=>{
      if(err) reject(err)
      else resolve(data)
    })
  })
}
readFileAsync('./package.json').then(data=>{
  data=JSON.parse(data)
  console.log(data.name)
}).catch(err=>{
  console.log(err)
})
```

# 使用 util.promisify
```javascript
const fs =require('fs')
const until =require('util')
until.promisify(fs.readFile)('./package.json').then(data=>{
  data=JSON.parse(data)
  console.log(data.name)
}).catch(err=>{
  console.log(err)
})
```

## 加上async 
```javascript
const fs = require('fs')
const until = require('util')
const readAsync = until.promisify(fs.readFile)
async function init() {
	try {
		let data = await readAsync('./package.json')
		data = JSON.parse(data)
		console.log(data.name)
	} catch (error) {
		console.log(error)
	}
}
init()

```