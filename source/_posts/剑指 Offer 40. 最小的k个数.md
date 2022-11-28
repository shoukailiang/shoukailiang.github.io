---
title: 剑指 Offer 40. 最小的k个数
date: 2021-01-02 19:55:02
tags: 
  - leetcode
  - 刷题
categories:
  - 算法
---

# 描述
输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4。
# 示例1
## 输入
```
[4,5,1,6,2,7,3,8],4
```
## 输出
```
[1,2,3,4]
```
# 思路
## 排序找（此处省略）
## 堆排序
可以采用大顶堆来实现。好了你要问了为什么是大顶堆不是小顶堆呢？不是找最小的k个数吗？
我们分析一下思路
首先将4个数放入数组中形成
```
4 5 1 6
```
然后既然是最小的k个数，那么最大的6就不要了，把6换成2
```
4 5 1 2 
```
如此循环，是不是很是熟悉，这不就是大顶堆的设计吗？
# 实现
这里采用java的优先队列来实现，但是java中的优先队列默认是小顶堆，所以需要实现比较器接口
```java
import java.util.*;
public class Solution {
    public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
        ArrayList<Integer> res = new ArrayList<>();
        int len = input.length;
        if(len<k||k==0){
            return res;
        }
        // java 中优先队列默认小顶堆
        // 这里用的lambda表达式实现比较器接口
        Queue<Integer> pq = new PriorityQueue<>(k,(a,b)->(b-a));

        for (int i = 0; i < len; i++) {
            if (pq.size()<k){
                pq.add(input[i]);
            }else {
                // 置换了
                if(input[i]<pq.peek()){
                    pq.poll();
                    pq.add(input[i]);
                }
            }
        }
        res.addAll(pq);
        return res;
        
    }
}
```