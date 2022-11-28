---
title: Leetcode67. 二进制求和
date: 2021-02-07 20:05:56
tags: 
  - leetcode
  - 刷题
categories:
  - 算法
---

你两个二进制字符串，返回它们的和（用二进制表示）。

输入为 非空 字符串且只包含数字 1 和 0。

 
```
示例 1:

输入: a = "11", b = "1"
输出: "100"
示例 2:

输入: a = "1010", b = "1011"
输出: "10101"
```

```cpp
class Solution {
public:
    string addBinary(string m, string n) {
            string m1;
        reverse(m.begin(),m.end());
        reverse(n.begin(),n.end());
        int count = 0;// 进位
        int m_len = m.size();
        int n_len = n.size();
        int max_size = max(m_len,n_len);
        int now;
        int i;
        for ( i = 0; i < max_size; ++i) {
            if(i<(m_len)&&(i<n_len)){
                 now = m[i]-'0'+n[i]-'0'+count;
            } else if(i<m_len){
                 now = m[i]-'0'+count;
                cout<<now<<endl;
            } else if(i<n_len){
                now = n[i]-'0'+count;
                cout<<now<<endl;
            }
            m1.push_back((now)%2+'0');
            count = (now)/2;
        }
        if(count==1){
            m1.push_back('1');
        }
        reverse(m1.begin(),m1.end());
        return m1;
    }
};
```
## 延伸一下
你的任务是：对于任意的两个正的二进制数，求它们和中“1”所在的位置。

Input
有多组测试数据，每组测试数据共两行：

第一行：二进制的加数A，总长度<=200。

第二行：二进制的加数B，总长度<=200。

Output
输出二进制数A+B中“1”所在的位置，位置排序为从左到右：N、N-1、N-2...1，数据之间用空格分隔，行末没有多余的空格。
```
Samples
input 
111
110
11101
110
output 
4 3 1
6 2 1
```

```cpp
#include <iostream>
#include "algorithm"
#include "cctype"
#include "string"
#include "stack"
#include "vector"
#include "unordered_set"
#include "unordered_map"
using namespace std;

int main() {
    string m, n;
    bool isPrint = false;
    while (cin>>m>>n){
        string m1;
        isPrint= false;
        reverse(m.begin(),m.end());
        reverse(n.begin(),n.end());
        int count = 0;// 进位
        int m_len = m.size();
        int n_len = n.size();
        int max_size = max(m_len,n_len);
        int now;
        int i;
        for ( i = 0; i < max_size; ++i) {
            if(i<(m_len)&&(i<n_len)){
                now = m[i]-'0'+n[i]-'0'+count;
            } else if(i<m_len){
                now = m[i]-'0'+count;
            } else if(i<n_len){
                now = n[i]-'0'+count;
            }
            m1.push_back((now)%2+'0');
            count = (now)/2;
        }
        if(count==1){
            m1.push_back('1');
        }
        reverse(m1.begin(),m1.end());
        for (int j = 0; j < m1.size(); ++j) {
            if(m1.at(j)=='1'&&isPrint== false){
                cout<<(m1.size()-j);
                isPrint= true;
            }else if (isPrint&&m1.at(j)=='1'){
                cout<<" ";
                cout<<(m1.size()-j);
            }
        }
        cout<<endl;
    }
}
```
