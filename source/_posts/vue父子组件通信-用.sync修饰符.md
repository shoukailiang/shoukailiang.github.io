---
title: vue父子组件通信-用.sync修饰符
date: 2021-04-19 20:42:26
tags: 
  - vue
categories:
 - 前端
---

# 原始方法
> App.vue
```js
<template>
  <div id="app">
    <Father :age="age" @setage="setAge"/>
  </div>
</template>

<script>
import Father from './components/Father.vue'

export default {
  data(){
    return{
      age:"18"
    }
  },
  name: 'app',
  components: {
    Father
  },
  methods:{
    setAge(res){
        this.age = res;
    }
  }
  
}
</script>

<style>

</style>

```
> Father.vue
```js
<template>
  <div class="hello">
    {{age}}
  </div>
</template>

<script>
export default {
  name: 'Father',
  props:{
    age:String
  },
  methods:{

  },
  mounted(){
    this.$emit("setage","123");
  }
  
}
</script>

<style scoped>

</style>

```
# 使用.sync修饰符
这里注意我们的事件名称被换成了update:age
```js
<template>
  <div class="hello">
    {{age}}
  </div>
</template>

<script>
export default {
  name: 'Father',
  props:{
    age:String
  },
  methods:{

  },
  mounted(){
    this.$emit("update:age","123");
  }
  
}
</script>

<style scoped>

</style>

```
