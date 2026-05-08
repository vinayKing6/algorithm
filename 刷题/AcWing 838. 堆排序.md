---
tags:
  - sort
  - heap
  - easy
date: 2026-03-07
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/840/

输入一个长度为 n 的整数数列，从小到大输出前 m 小的数。

#### 输入格式

第一行包含整数 n 和 m。

第二行包含 n 个整数，表示整数数列。

#### 输出格式

共一行，包含 m 个整数，表示整数数列中前 m 小的数。

#### 数据范围

1≤m≤n≤$10^5$，  
1≤数列中元素≤$10^9$

#### 输入样例：

```
5 3
4 5 1 3 2
```

#### 输出样例：

```
1 2 3
```

# 思路

用priority_queue

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+10;
int n,k;
priority_queue<int,vector<int>,greater<int>> q;//小根堆

int main(){
    cin>>n>>k;
    for(int i=0;i<n;i++){
        int a;
        cin>>a;
        q.push(a);
    }
    
    for(int i=0;i<k-1;i++){
        cout<<q.top()<<" ";
        q.pop();
    }
    cout<<q.top()<<endl;
}
```