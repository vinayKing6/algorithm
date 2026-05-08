---
tags:
  - easy
  - 位运算
date: 2026-03-09
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/803/

给定一个长度为 n 的数列，请你求出数列中每个数的二进制表示中 1 的个数。

#### 输入格式

第一行包含整数 n。

第二行包含 n 个整数，表示整个数列。

#### 输出格式

共一行，包含 n 个整数，其中的第 i 个数表示数列中的第 i 个数的二进制表示中 1 的个数。

#### 数据范围

1≤n≤100000,  
0≤数列中元素的值≤$10^9$

#### 输入样例：

```
5
1 2 3 4 5
```

#### 输出样例：

```
1 1 2 1 2
```

# 思路

使用lowbit

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

int n;  // 输入整数的个数

// lowbit函数：返回x的二进制表示中最低位的1所对应的值
// 例如：
// x = 12 (1100) -> lowbit = 4 (0100)
// x = 10 (1010) -> lowbit = 2 (0010)
int lowbit(int x){
    return x & -x;   // 利用补码性质提取最低位1
}

int main(){
    cin >> n;  // 输入n个整数

    // 处理前 n-1 个数
    for(int i = 0; i < n - 1; i++){
        int a;
        int zero = 0;   // 记录二进制中1的个数

        cin >> a;       // 输入整数a

        // 利用lowbit逐次去掉最低位的1
        // 每循环一次说明存在一个1
        while(a){
            int t = lowbit(a); // 取出最低位的1
            if(t) zero++;      // 统计1的个数
            a -= t;            // 去掉这个1
        }

        cout << zero << " ";   // 输出该数二进制中1的个数
    }

    // 单独处理最后一个数（为了输出格式不多空格）
    int a, zero = 0;
    cin >> a;

    while(a){
        int t = lowbit(a); // 取最低位1
        if(t) zero++;      // 统计
        a -= t;            // 去掉最低位1
    }

    cout << zero << endl;  // 输出最后一个结果并换行
}
```