---
title: 寻找第K大数
date: 2020-12-31 15:42:32
tags: 
  - leetcode
  - 刷题
categories:
  - 算法
---


# 题目描述
有一个整数数组，请你根据快速排序的思路，找出数组中第K大的数。

给定一个整数数组a,同时给定它的大小n和要找的K(K在1到n之间)，请返回第K大的数，保证答案存在。
# 示例
```
[1,3,5,2,2],5,3
```
# 返回
2
# 思路
利用快排，每次排序后，将确定位置的数的下标与k-1比较
- 若相等：返回
- 若大于k-1,则在左半部分递归查找
- 如小于k-1,则在右半部分递归查找
# 代码
```java
import java.util.*;

public class Finder {
    public int findKth(int[] a, int n, int K) {
        return findK(a,0,n-1,K);
    }
    int partition(int[] a,int left,int right){
        int pivot = a[left];
        while(left<right){
            while(left<right&&a[right]<pivot){
                right--;
            }
            a[left] = a[right];
            while(left<right&&a[left]>pivot){
                left++;
            }
            a[right] = a[left];
        }
        a[left] = pivot;
        return left;
    }
    int findK(int[] a,int left,int right,int k){
        int pivot = partition(a,left,right);
        if(pivot==k-1){
            return a[pivot];
        }
        if(pivot>k-1){
            return findK(a,left,pivot-1,k);
        }else {
            return findK(a,pivot+1,right,k);
        }
    }
}
```

