---
title: LeetCode刷题0001_two-sum
date: 2019-06-01 21:54:07
tags: 
  - java
  - 算法
  - 数据结构
  - leetcode
categories:
 - 算法
 - 数据结构
 - leetcode
---
# 题目要求
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

### Example:
```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

# 方法一：暴力破解法
暴力法很简单，遍历每个元素 xx，并查找是否存在一个值与 target−x 相等的目标元素。
## 复杂度分析
- 时间复杂度：O(n^2)
- 空间复杂度：O(1)
## 代码实现java
```java
class Solution {
  public int[] twoSum(int[] nums, int target) {
    for(int i = 0 ; i < nums.length; i ++){
      for(int j = 0 ; j < nums.length ; j ++){
        if(nums[i] + nums[j] == target){
          int[] res = {i, j};
          return res;
        }
      }
    }
    throw new IllegalStateException("the input has no solution");
  }
}
```
# 方法二：哈希表
使用HashMap,基本思路是：用数组的值作为key，index作为value。对数组进行迭代的时候，将元素插入到hashMap中的，这时我们回过头来检查表中是否已经存在当前元素所对应的目标元素。如果它存在，那我们已经找到了对应解，并立即将其返回。
## 复杂度分析
- 时间复杂度：O(n)
- 空间复杂度：O(n)， 所需的额外空间取决于哈希表中存储的元素数量，该表最多需要存储 nn 个元素。
## java代码实现
```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        HashMap<Integer,Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int temp =target-nums[i];
            if(map.containsKey(temp)){  // 返回布尔
                return new int []{map.get(temp),i};
            }
            map.put(nums[i],i);   // key value
        }
        throw new IllegalStateException("the input has no solution");
    }
}
```
