---
title: ForkJoin知识点提要
date: 2021-08-13 09:18:28
tags: 
  - java
  - 高并发
categories:
 - 后端
---

一句话总结:把大任务拆分为小任务

![image.png](https://shoukailiang-blog.oss-cn-hangzhou.aliyuncs.com/article/202212031248820.png)
# demo
1加到10_0000_0000
```java

package com.company;

import java.util.concurrent.RecursiveTask;

/**
 * @author shoukailiang
 * @version 1.0
 * @date 2021/8/8 17:21
 */
public class ForkJoinDemo extends RecursiveTask<Long> {

    private Long start;
    private Long end;

    // 临界值
    private Long temp = 10000L;

    public ForkJoinDemo(Long start, Long end) {
        this.start = start;
        this.end = end;
    }

    @Override
    protected Long compute() {
        if ((end-start)<temp){
            Long sum = 0L;
            for (Long i = start; i <= end; i++) {
                sum += i;
            }
            return sum;
        }else{
            // forkjoin 递归
            long middle = (start + end) / 2; // 中间值
            ForkJoinDemo task1 = new ForkJoinDemo(start, middle);
            task1.fork(); // 拆分任务，把任务压入线程队列
            ForkJoinDemo task2 = new ForkJoinDemo(middle+1, end);
            task2.fork(); // 拆分任务，把任务压入线程队列
            return task1.join() + task2.join();
        }
    }
}
```
# 三种方法对比测试
```java
class TestFork{
    // 循环计算
    public static void test1(){
        Long sum = 0L;
        long start = System.currentTimeMillis();
        for (Long i = 1L; i <= 10_0000_0000; i++) {
            sum += i;
        }
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+" 时间："+(end-start));
    }
    //ForkJoin
    public static void test2() throws ExecutionException, InterruptedException {
        long start = System.currentTimeMillis();
        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinDemo forkJoinDemo = new ForkJoinDemo(0L, 10_0000_0000L);
        ForkJoinTask<Long> submit = forkJoinPool.submit(forkJoinDemo);
        Long sum  = submit.get();// 获得结果
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+" 时间："+(end-start));
    }
    // 并行流
    public static void test3(){
        long start = System.currentTimeMillis();
        // Stream并行流 ()  (]
        long sum = LongStream
                .rangeClosed(0L, 10_0000_0000L) // 计算范围(,]
                .parallel() // 并行计算
                .reduce(0, Long::sum); // 输出结果

        long end = System.currentTimeMillis();
        System.out.println("sum="+"时间："+(end-start));
    }
    public static void main(String[] args) throws ExecutionException, InterruptedException {
         test1();
         test2();
         test3();
    }
}
```
结果
```
sum=500000000500000000 时间：6776
sum=500000000500000000 时间：5497
sum=500000000500000000 时间：79
```