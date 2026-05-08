---
tags:
  - trie
  - 二进制
  - mid
date: 2026-03-09
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/145/

在给定的 N 个整数 A1，A2……AN 中选出两个进行 xor（异或）运算，得到的结果最大是多少？

#### 输入格式

第一行输入一个整数 N。

第二行输入 N 个整数 A1～AN。

#### 输出格式

输出一个整数表示答案。

#### 数据范围

1≤N≤$10^5$,  
0≤Ai<$2^{31}$

#### 输入样例：

```
3
1 2 3
```

#### 输出样例：

```
3
```

# 思路

**思路：**  
用Trie（字典树）。

- 建树时，根据输入数字的对应的二进制串构造一个 Trie 树。
    
- Trie 树的每个结点两个分支，分支指向的两个son结点分别表示当前位的数值为0或1
    
- 记录每次输入的数字转化成的二进制串，当前位为1，就走到数值为1的结点，否则走到0结点
    
- 这样每个数字对应的Trie中的路径就是唯一的。
    

因为要求异或对的值最大，可以用贪心的思想。

- 在第一个数字固定的情况下，尽可能地让第二个数的每一位都与第一个数的对应位相反，这样最终确定的第二个数与第一个数的异或值就最大，所以在查询时，遍历第一个串o（n），根据固定的第一个二进制串，每次尽可能走到与当前位的值相反的结点，这样的路径对应的就是与第一个二进制串异或值最大的二进制串，便利了这个数的位数次o（logn），所以总的时间复杂度o（n*logn）；

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;

// trie[i][0] 表示第 i 个节点的 0 分支
// trie[i][1] 表示第 i 个节点的 1 分支
int trie[N * 31][2];

int n;        // 输入数字个数
int idx;      // trie节点编号
int a[N];     // 存储所有输入的数


// 向Trie中插入一个整数x（二进制形式）
void insert(int x){
    int p = 0;   // 从根节点开始

    // 从最高位到最低位（32位整数）
    for(int i = 31; i >= 0; i--){
        int u = (x >> i) & 1;   // 取出第 i 位（二进制）

        // 如果当前位对应的节点不存在，就新建节点
        if(!trie[p][u])
            trie[p][u] = ++idx;

        // 移动到下一层
        p = trie[p][u];
    }
}


// 在Trie中查询与x异或值最大的数
int querry(int x){
    int ret = 0;  // 用来记录找到的数
    int p = 0;    // 从Trie根节点开始

    for(int i = 31; i >= 0; i--){
        int u = (x >> i) & 1;  // 取出x的第i位

        // 为了使异或最大，优先选择相反的位
        if(trie[p][!u]){
            ret = ret * 2 + !u;     // 构造找到的数
            p = trie[p][!u];        // 走相反位
        }
        else{
            ret = ret * 2 + u;      // 只能走相同位
            p = trie[p][u];
        }
    }

    // ret 是Trie中找到的数
    // 返回 x ^ ret 即为最大异或值
    return ret ^ x;
}


int main(){
    cin >> n;

    // 输入所有数并插入Trie
    for(int i = 0; i < n; i++){
        int x;
        cin >> x;

        insert(x);   // 插入Trie
        a[i] = x;    // 保存原数组
    }

    int maxor = 0;  // 最大异或值

    // 枚举每个数，寻找和它异或最大的数
    for(int i = 0; i < n; i++){
        maxor = max(maxor, querry(a[i]));
    }

    // 输出最大异或值
    cout << maxor << endl;
}
```