---
title: CyclicBarrier知识点提要
date: 2021-08-13 15:15:52
tags: 
  - java
  - CyclicBarrier
categories:
 - 后端
---

# CyclicBarrier

## 概念

和CountDownLatch相反，需要集齐七颗龙珠，召唤神龙。也就是做加法，开始是0，加到某个值的时候就执行

CyclicBarrier的字面意思就是可循环（cyclic）使用的屏障（Barrier）。它要求做的事情是，让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活，线程进入屏障通过CyclicBarrier的await方法

## 案例

集齐7个龙珠，召唤神龙的Demo，我们需要首先创建CyclicBarrier

```java
 /**
 * 定义一个循环屏障，参数1：需要累加的值，参数2 需要执行的方法
 */
 CyclicBarrier cyclicBarrier = new CyclicBarrier(7, () -> {
   System.out.println("召唤神龙许愿---------------");
 });
```

然后同时编写七个线程，进行龙珠收集，但一个线程收集到了的时候，我们需要让他执行await方法，等待到7个线程全部执行完毕后，我们就执行原来定义好的方法

```java
for (int i = 0; i < 7; i++) {
    final Integer temp = i;
    new Thread(() -> {
        System.out.println(Thread.currentThread().getName() + "\t 收集到 第" + temp + "颗龙珠");
​
        try {
            // 先到的被阻塞，等全部线程完成后，才能执行方法
            cyclicBarrier.await();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (BrokenBarrierException e) {
            e.printStackTrace();
        }
    }, String.valueOf(i)).start();
}
```

完整代码

```java
 package com.company;
 ​
 import java.util.concurrent.BrokenBarrierException;
 import java.util.concurrent.CyclicBarrier;
 ​
 /**
  * @author shoukailiang
  * @version 1.0
  * @date 2021/8/7 18:33
  */
 public class CyclicBarrierDemo {
     public static void main(String[] args) {
         CyclicBarrier cyclicBarrier = new CyclicBarrier(7, () -> {
             System.out.println("召唤神龙许愿---------------");
         });
         for (int i = 0; i < 7; i++) {
             final Integer temp = i;
             new Thread(()->{
                 System.out.println(Thread.currentThread().getName() + "\t 收集到第" + temp + "颗龙珠");
 ​
                 try {
                     cyclicBarrier.await();
                 } catch (InterruptedException e) {
                     e.printStackTrace();
                 } catch (BrokenBarrierException e) {
                     e.printStackTrace();
                 }
             },String.valueOf(i)).start();
         }
     }
 }
 ​
```

\