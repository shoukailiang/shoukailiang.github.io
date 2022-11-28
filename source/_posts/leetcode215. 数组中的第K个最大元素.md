---
title: leetcode215. 数组中的第K个最大元素
date: 2021-02-12 13:27:15
tags: 
  - leetcode
  - 刷题
categories:
  - 算法
---

这题和剑指 Offer 40. 最小的k个数思路基本相同。
> 思路:用一个小顶堆，筛选出k个最大的元素，那么在头的那个就是和第k大的元素了

```c
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        // 小顶堆
        priority_queue<int,vector<int>,greater<int>> pq;
        // priority_queue<int,vector<int>,less<int>> pq2;
        for (int i = 0; i < k; ++i) {
            pq.push(nums[i]);
        }
        for (int i = k; i <nums.size() ; ++i) {
            if(nums[i]>pq.top()){
                pq.pop();
                pq.push(nums[i]);
            }
        }
        return pq.top();
    }
};
```
