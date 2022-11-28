---
title: js作用域（变量提升，预解析）例题
date: 2018-03-10 19:49:51
tags: 
  - 前端
  - javascript
categories:
  - 前端
---

### 典型例题如下：

```javascript
  alert(a)   // function
  a()    //10
  var a=3;
  function a() {
    alert(10)
  }
  alert(a) //3
  a=6;
  a()  // 没有a这个函数了啊
```
> 为什么会有这样的结果呢？请看下面的例题

### 例题一
```javascript
  /*1
  alert(a); //underfined（未定义）
  var a=1;
  */

  /*2
  alert(a)
  a=1; //报错,a is not defined
  */
  
  /*
  
  js解析器
  1.“找一些东西”（js预解析）var function 参数
   根据var找到； a=未定义，所有的变量在正式运行前都提前都赋值了未定义
   fn1=function fn1() {alert(2)}
   所有的函数，在正式运行代码之前，都是整个函数快
  2.逐行解读代码
    表达式：= + - * 、/ % ++ --! 参数 .......

   alert(a)
   var a=1;
   function fn1() {alert(2)}
   */
```

### 例题二

```javascript
  /*
  js预解析只会留下一个，！！！变量和函数重名了，只留下函数 
  第一步：
  a=未定义
  a=function a() {alert(2)}，那么上面的那个就被干掉了；
  最后，在预解析仓库里面只有：
   a=function a() {alert(4)}
   */

  //开始逐行解读
  alert(a);//function a() {alert(4)}
  var a=1;//表达式：a就被改成1了;
  alert(a)//1
  function a() {alert(2)}//是函数声明，不是表达式，不会改变a的值
  alert(a);//1
  var a=3;
  alert(a);//3
  function a() {alert(4)};
  alert(a);//3

  a()//报错，读完代码之后，js仓库里面只有a=3;
```

### 例题三

```javascript
<!--<script>-->
    <!--//1.预解析-->
    <!--//2.执行-->
<!--</script>-->

<!--<script>-->
    <!--//3.预解析-->
    <!--//4.执行-->
<!--</script>-->

 <!--
 1.
  <script>
    var a=3;
  </script>

  <script>
    alert(a);//3
  </script>
 -->

  <!--
  2. <script>
      alert(a);//报错；a is not defined
    </script>

    <script>
      var a=3;
    </script>

  -->

```

### 例题四

```javascript
  var a=1;
  function fn1() {
    alert(a);//undefined
    var a=2;
  }
  fn1();
  alert(a);//1
  /*
  1.预解析
  全局的   a=未定义
  fn1=function fn1() {alert(a);var a=2;}
  2.逐行解读代码:
  表达式 a=1;
  函数调用
    1）预解析
    局部的   a =未定义

    2）逐行解读代码
    alert(a)=>未定义
          a=2;
   */
```

### 例题4-1

```javascript
  var a=1;
  function fn1() {
    alert(a);//1
     a=2;
  }
  fn1();
  alert(a);//2
  /*
  1.预解析
  全局的   a=未定义
  fn1=function fn1() {alert(a); a=2;}
  2.逐行解读代码:
  表达式 a=1;
  函数调用
    1）预解析
    顺着这一层的作用域跳到上一层去找（作用域链）

    2）逐行解读代码
    alert(a)//当前作用域没有，顺着这一层的作用域跳到上一层去找（作用域链）
    a=2也是
    先弹出1,再弹出2
   */

```

### 例题4-2

```javascript
  var a=1;
  function fn1(a) {//参数相当于是一个局部变量   相当于括号里面是var a;
    alert(a);//undefined
     a=2;
  }
  fn1();
  alert(a);//1
  /*
  1.预解析
     a=未定义
  fn1=function fn1(a) {alert(a); a=2;}
  2.逐行解读代码:
  表达式 a=1;
  函数调用
    1）预解析
    里面的a=未定义

    2）逐行解读代码
          里面的a=2;
   */
```

### 例题4-3

```javascript
  var a=1;//标志1
  function fn1(a) {//参数相当于是一个局部变量   相当于括号里面是var a;
    alert(a);//1,这个a和标志1处的a是不相同的,这个a是局部的，外面的a是全局的
    a=2;
    alert(a)//2
  }
  fn1(a);//这个a是全局
  alert(a);//1
  /*
  1.预解析
     a=未定义
  fn1=function fn1(a) {alert(a); a=2;}
  2.逐行解读代码:
  表达式 a=1;
  函数调用
    1）预解析
    里面的a=未定义

    2）逐行解读代码
          读到fn1(a)的时候  就function fn1(var a=1)
          找到局部的a=2
   */
```

### 例题4-4

```javascript
  var a=1;
  function fn1(a) {       //相当于var a;a=1(外面的)
    alert(a);//1
     var a=2;
     console.log(a);//2
  }
  fn1(a);
  alert(a);//1

```

### 例题4-5

```javascript
  var a=1;
  function fn1(a) { 
    arguments[0]=3;
    alert(a);//3
    var a=2;
    console.log(a);//2
  }
  fn1(a);
  alert(a);//1

```

### 例题5 想要获取函数内的值

```javascript
  //想要获取函数内的值
  // 一
  /*
  var str=''
  function fn1() {
    var a='1';
    str=a;
  }
  fn1();
  alert(str)
  */
  // 二
  function fn2() {
    var a='100元';
    fn3(a);
  }
  fn2()
  function fn3(b) {
    alert(b)
  }

```

### 例题6

```javascript
  /*
  函数的大括号才是一个域
  if条件判断的大括号不是一个作用域，var 写在大括号里面和外面是一样的
   if(1){
   var a=1;
   }
   alert(a);
   for 循环的{}也不是一个块级作用域

  是作用域的标志是先解析后执行
   */


  /*
  alert(a);//undefined
  if(true){
    var a=1;
  }*/

  /*
  alert( fn1 );//underfine(最新的浏览器)，ie10及以下是可以弹出函数的。。。
  if(true){
    var a=1;
    function fn1() {
      alert(123)
    }
  }*/


  /*
  //所以，以上代码要写成
  alert( fn1 );
  var a=1;
  function fn1() {
    alert(123)
  }
  if(true){

  }*/

```

### 例题7
```html
<input type="button" value="1">
<input type="button" value="2">
<input type="button" value="3">
```

```javascript

    /*
    //1.
    window.onload=function () {
      var oBtn=document.getElementsByTagName('input');
      for(var i=0;i<oBtn.length;i++){
        oBtn[i].onclick=function () {
          console.log(i);//3
        }
      }
    }*/

    
/*     //2
    window.onload=function () {
      var oBtn=document.getElementsByTagName('input');
      for(var i=0;i<oBtn.length;i++){
        oBtn[i].onclick=function () {
          console.log(i);//undefined     因为函数域解析了,来自下面for的var i;
          for(var i=0;i<oBtn.length;i++){
            oBtn[i].style.background='yellow'
          }
        }
      }
    } */
```
