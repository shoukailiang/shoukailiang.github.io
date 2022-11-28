---
title: leetcode hashmap入门题
date: 2021-02-19 23:05:12
tags: 
  - 算法
  - hashmap
  - leetcode
categories:
 - 算法
---

# 1. 两数之和
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素不能使用两遍。

你可以按任意顺序返回答案。


### Example:
```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。

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
# 2.和为S的两个数字(plus版本)
题目描述
输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

返回值描述:
对应每个测试案例，输出两个数，小的先输出。

示例1
```
输入

[1,2,4,7,11,15],15
返回值

[4,11]
```
```cpp
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array,int sum) {
        unordered_map<int,int> maps;
        int temp = INT_MAX;
        vector<int> ret;
        for (int i=0; i<array.size(); ++i) {
            maps[array[i]] = i;
        }
        for(int i =0;i<array.size();i++){
            int complement = sum-array[i];
            if(maps.find(complement)!=maps.end()){
                if(complement*array[i]<temp){
                    ret = {array[i],complement};
                }
            }
             maps[complement] = i;
        }
        if(ret.size()!=0){
            sort(ret.begin(), ret.end());
            return ret;
        }
        return vector<int>{};
    }
};
```