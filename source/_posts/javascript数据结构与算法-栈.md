---
title: javascript数据结构与算法-栈
date: 2018-11-13 10:24:58
tags: 
  - javascript
  - typescript
  - 前端
  - 数据结构
categories:
 - 前端
 - 数据结构
---

# 定义
栈作为一种数据结构，是一种只能在一端进行插入和删除操作的特殊线性表。它按照先进后出的原则存储数据，先进入的数据被压入栈底，最后的数据在栈顶，需要读数据的时候从栈顶开始弹出数据（最后一个数据被第一个读出来）。栈具有记忆作用，对栈的插入与删除操作中，不需要改变栈底指针。
栈是允许在同一端进行插入和删除操作的特殊线性表。允许进行插入和删除操作的一端称为栈顶(top)，另一端为栈底(bottom)；栈底固定，而栈顶浮动；栈中元素个数为零时称为空栈。插入一般称为进栈（PUSH），删除则称为退栈（POP）。栈也称为后进先出表。
# 方法和属性
- push方法 入栈
- pop方法  删除栈顶的元素
- peek方法 查看当前栈顶的元素
- isEmpty方法
- size方法
- toString方法 打印出所有元素
- length属性 栈的元素个数
- list属性  存储栈  
下面是出栈和入栈的示意图，照片来自网络
![](https://user-gold-cdn.xitu.io/2018/11/14/1671023fd9b06701?w=389&h=270&f=jpeg&s=9410)
# 代码实现栈
这里我们用js数组来模拟栈，应为js是一门强大的高级语言，数组的push和pop方法在栈中也同样适用
```
class Stack {
  constructor() {
    this.list = [];
    this.length = 0;
  }
  push(value) {
    this.length++;
    this.list.push(value);
  }
  pop() {
    if (this.isEmpty()) {
      return undefined;
    }
    this.length--;
    return this.list.pop();
  }
  isEmpty() {
    return this.length === 0;   
  }
  size() {
    return this.length;
  }
  peek() {
    if (this.isEmpty()) {
      return undefined;
    }
    return this.list[this.length - 1];
  }
  toString() {
    if (this.isEmpty()) {
      return '';
    }
    let objString = `${this.list[0]}`;
    for (let i = 1; i < this.length; i++) {
      objString = `${objString},${this.list[i]}`;
    }
    return objString;
  }
}
```
# 或者用对象来实现栈，但是没有数组原生的那些函数了
入栈的时候，索引会变成对象的下标，就能set和get了,删除用delete
```
class Stack {
  constructor() {
    this.length = 0;
    this.items = {};
  }
  push(element) {
    this.items[this.length] = element;
    this.length++;
  }
  pop() {
    if (this.isEmpty()) {
      return undefined;
    }
    this.length--;
    const result = this.items[this.length];
    delete this.items[this.length];
    return result;
  }
  peek() {
    if (this.isEmpty()) {
      return undefined;
    }
    return this.items[this.length - 1];
  }
  isEmpty() {
    return this.length === 0;
  }
  size() {
    return this.length;
  }
  clear() {
    /* while (!this.isEmpty()) {
        this.pop();
      } */
    this.items = {};
    this.length = 0;
  }
  toString() {
    if (this.isEmpty()) {
      return '';
    }
    let objString = `${this.items[0]}`;
    for (let i = 1; i < this.length; i++) {
      objString = `${objString},${this.items[i]}`;
    }
    return objString;
  }
}
```
# 用ts实现一遍
```
class Stack<T> {
  private count: number;
  private items: any;

  constructor() {
    this.count = 0;
    this.items = {};
  }

  push(element: T) {
    this.items[this.count] = element;
    this.count++;
  }

  pop() {
    if (this.isEmpty()) {
      return undefined;
    }
    this.count--;
    const result = this.items[this.count];
    delete this.items[this.count];
    return result;
  }

  peek() {
    if (this.isEmpty()) {
      return undefined;
    }
    return this.items[this.count - 1];
  }

  isEmpty() {
    return this.count === 0;
  }

  size() {
    return this.count;
  }

  clear() {
    /* while (!this.isEmpty()) {
      this.pop();
    } */

    this.items = {};
    this.count = 0;
  }
  toString() {
    if (this.isEmpty()) {
      return '';
    }
    let objString = `${this.items[0]}`;
    for (let i = 1; i < this.count; i++) {
      objString = `${objString},${this.items[i]}`;
    }
    return objString;
  }
}
```
# 存在的问题
尽管代码看起来还行，但是我们发现list时公用的，es6貌似不能声明私有变量和私有函数,毕竟别的语言的 private 太强大了,网上有很多种实现形式，目前比较让人认可的，主要是weakmap和symbol两种，但我觉得这样写代码也太不优雅了，先公共着吧，hhhhh
# 用栈实现10进制向n进制转换
我们知道二进制是除2取余再从下往上拿余数，取余数是%，从下往上拿余数和从循环栈顶拿元素相似。同理转8 进制就是除8取余....
实现思路：
- 最高位为 num % base 然后直接压入栈;
- 使用 num / base 来代替 num ;
- 重复上面的步骤，直到 n 为 0 ，并且没有余数；
- 以此将栈内元素弹出，直到栈空，并依次将这些元素排列，就得到了转换后的形式
```
let mulBase =(num,base)=>{
    let s = new Stack();
    while(num>0){
        s.push(num%base);
        num = Math.floor(num/=base);
    }
    var converted = "";
    while(s.size()>0){
        converted+=s.pop();
    }
    return converted;
} 
```

# 用栈实现回文判断
实现思路：将字符串的每一次一次入栈，探后循环出栈，判断出栈后的字符串和原来的字符串是否是相等的，若一致，则是回文
```
let isPalindrome=(str)=>{
    let s = new Stack();
    for(let i = 0;i<str.length;i++){
        // 依次入栈
        s.push(str[i]);
    }
    let newStr = "";
    while(s.size()>0){
        newStr+=s.pop();
    }
    if(newStr===str){
        return true;
    }else{
        return false;
    }
}
console.log(isPalindrome("123321")) // true
```
另一种方法字符串直接翻转就好了
```
 let isPalindrome =( word )=>{
    return String(word).split('').reverse().join('') == word ? true : false;
}
```