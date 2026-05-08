---
tags:
  - dp
  - 背包问题
  - mid
date: 2026-04-26
练习次数: 2
---
# 题目

https://www.acwing.com/problem/content/280/

给定 N 个正整数 A1,A2,…,AN，从中选出若干个数，使它们的和为 M，求有多少种选择方案。

#### 输入格式

第一行包含两个整数 N 和 M。

第二行包含 N 个整数，表示 A1,A2,…,AN。

#### 输出格式

包含一个整数，表示可选方案数。

#### 数据范围

1≤N≤100,  
1≤M≤10000,  
1≤Ai≤1000,  
答案保证在 int 范围内。

#### 输入样例：

```
4 4
1 1 2 2
```

#### 输出样例：

```
3
```

# 思路

进阶:[[AcWing 1022. 宠物小精灵之收服]]

优先dfs,但会超时

![[Pasted image 20260314123047.png]]
对比一下:[[AcWing 2.01背包问题]]
未解决疑问: 为什么此题的`f[i][j]`是恰好拼成j,而01背包是不超过j?????????不理解
类似题[[sunwhy 有效混合总质量]]

# 代码

```c++
#include <iostream>
#include <algorithm>

using namespace std;

const int N = 10010;

int n, m;
int f[N];

int main()
{
    cin >> n >> m;

    f[0] = 1;
    for (int i = 0; i < n; i ++ )
    {
        int v;
        cin >> v;
        for (int j = m; j >= v; j -- )
            f[j] += f[j - v];
    }

    cout << f[m] << endl;

    return 0;
}

作者：yxc
链接：https://www.acwing.com/activity/content/code/content/117226/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```