---
title: LeetCode刷题0206_reverse-linked-list
date: 2019-06-02 14:21:59
tags: 
  - c++
  - 算法
  - 数据结构
  - leetcode
categories:
 - 算法
 - 数据结构
 - leetcode
---
# 题目要求
Reverse a singly linked list.
#### Example:
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```

# 方法一:迭代

![](https://user-gold-cdn.xitu.io/2019/6/2/16b171160b8a2c9e?w=725&h=540&f=png&s=172835)

## 代码实现（cpp）
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val; // 数据域
 *     ListNode *next;  //指针域
 *     ListNode(int x) : val(x), next(NULL) {} //构造函数
 * };
 */
class Solution {
public:
  ListNode* reverseList(ListNode* head) {
    ListNode *new_head = NULL;  //指向新链表头结点的指针
    while (head){
      ListNode *next = head->next;// 备份head->next
      head->next = new_head;//更新head->next
      new_head = head;// 移动new_head
      head=next; // 遍历链表
    }
    return new_head;
  }
};
```

