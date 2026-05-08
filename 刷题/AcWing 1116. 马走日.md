---
tags:
  - graph
  - dfs
  - 回溯
  - easy
date: 2026-03-07
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/1118/

马在中国象棋以日字形规则移动。

请编写一段程序，给定 n∗m 大小的棋盘，以及马的初始位置 (x,y)，要求不能重复经过棋盘上的同一个点，计算马可以有多少途径遍历棋盘上的所有点。

#### 输入格式

第一行为整数 T，表示测试数据组数。

每一组测试数据包含一行，为四个整数，分别为棋盘的大小以及初始位置坐标 n,m,x,y。

#### 输出格式

每组测试数据包含一行，为一个整数，表示马能遍历棋盘的途径总数，若无法遍历棋盘上的所有点则输出 0。

#### 数据范围

1≤T≤9,  
1≤m,n≤9,  
1≤n×m≤28,  
0≤x≤n−1,  
0≤y≤m−1

#### 输入样例：

```
1
5 4 0 0
```

#### 输出样例：

```
32
```

# 思路

参考[[AcWing 843. n-皇后问题]]

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

// 定义棋盘大小的最大边界，N=10 意味着棋盘不超过 10x10
const int N = 10;
int T, n, m, st[N][N], x, y, cnt, res;

// 骑士（马）走“日”字的 8 种方向偏移量
int dx[] = {2, 2, -2, -2, 1, 1, -1, -1};
int dy[] = {1, -1, 1, -1, 2, -2, 2, -2};

/**
 * DFS 搜索函数
 * @param cnt: 剩余需要访问的格子数量
 * @param x, y: 当前马所在的位置
 */
void dfs(int cnt, int x, int y) {
    // 递归出口：如果剩余步数为 0，说明所有格子都走过了，找到一条有效路径
    if (cnt == 0) {
        res++;
        return;
    }
    
    // 遍历骑士的 8 种走法
    for (int i = 0; i < 8; i++) {
        int xx = x + dx[i];
        int yy = y + dy[i];
        
        // 1. 边界检查：防止跳出棋盘
        if (xx < 0 || xx >= n || yy < 0 || yy >= m) continue;
        // 2. 状态检查：如果格子已被访问过，跳过
        if (st[xx][yy]) continue;
        
        // 3. 标记当前格子为已访问
        st[xx][yy] = 1;
        // 4. 递归搜索下一层，cnt 减 1
        dfs(cnt - 1, xx, yy);
        // 5. 回溯：恢复状态，尝试其他路径
        st[xx][yy] = 0;
    }
}

int main() {
    // 优化输入输出
    ios::sync_with_stdio(false);
    cin.tie(0);

    cin >> T; // 读取测试组数
    while (T--) {
        // 清空棋盘访问状态
        memset(st, 0, sizeof st);
        // 读取棋盘行列数 n, m 和起始位置 x, y
        cin >> n >> m >> x >> y;
        
        res = 0; // 重置路径统计数
        
        // 标记起点为已访问
        st[x][y] = 1;
        
        // 从起点出发，开始递归遍历，起始步数设为总格子数 n*m - 1
        dfs(n * m - 1, x, y);
        
        cout << res << endl;
    }
}
```