---
tags:
  - graph
  - bfs
  - 最短路
  - easy
date: 2026-03-05
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/1078/

给定一个 n×n 的二维数组，如下所示：

```
int maze[5][5] = {

0, 1, 0, 0, 0,

0, 1, 0, 1, 0,

0, 0, 0, 0, 0,

0, 1, 1, 1, 0,

0, 0, 0, 1, 0,

};
```

它表示一个迷宫，其中的1表示墙壁，0表示可以走的路，只能横着走或竖着走，不能斜着走，要求编程序找出从左上角到右下角的最短路线。

数据保证至少存在一条从左上角走到右下角的路径。

#### 输入格式

第一行包含整数 n。

接下来 n 行，每行包含 n 个整数 0 或 1，表示迷宫。

#### 输出格式

输出从左上角到右下角的最短路线，如果答案不唯一，输出任意一条路径均可。

按顺序，每行输出一个路径中经过的单元格的坐标，左上角坐标为 (0,0)，右下角坐标为 (n−1,n−1)。

#### 数据范围

0≤n≤1000

#### 输入样例：

```
5
0 1 0 0 0
0 1 0 1 0
0 0 0 0 0
0 1 1 1 0
0 0 0 1 0
```

#### 输出样例：

```
0 0
1 0
2 0
2 1
2 2
2 3
2 4
3 4
4 4
```

# 思路

复习: [[AcWing 844. 走迷宫]]

思路一致,只是增加路径,用`path[N][N]`就可以了,然后从终点向前搜

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 1010;
int n, g[N][N], st[N][N]; // g: 迷宫地图, st: 标记是否访问过
typedef pair<int, int> PII;
queue<PII> q;             // BFS 队列
PII path[N][N];           // 存储路径，path[i][j] 表示 (i,j) 是从哪个坐标走过来的

// 偏移量数组：上、下、左、右
int dx[4] = {0, 1, 0, -1};
int dy[4] = {1, 0, -1, 0};

void bfs() {
    // 【核心策略】从终点 (n-1, n-1) 开始搜索
    q.push({n - 1, n - 1});
    st[n - 1][n - 1] = 1;
    
    while (!q.empty()) {
        PII u = q.front();
        q.pop();
        
        int x = u.first;
        int y = u.second;
        
        // 如果已经搜到了起点 (0,0)，可以提前结束（此时最短路已找到）
        if (x == 0 && y == 0) break;
        
        for (int i = 0; i < 4; i++) {
            int xx = x + dx[i];
            int yy = y + dy[i];
            
            // 越界检查
            if (xx < 0 || xx >= n || yy < 0 || yy >= n) continue;
            // 已访问检查
            if (st[xx][yy]) continue;
            // 障碍物检查 (1 代表墙)
            if (g[xx][yy] == 1) continue;
            
            q.push({xx, yy});
            st[xx][yy] = 1;
            // 记录路径：点 (xx, yy) 是由当前点 (x, y) 扩展出来的
            // 因为是从终点往回搜，所以 path[起点] 指向的是它通往终点的第一步
            path[xx][yy] = {x, y};
        }
    }
    
    // --- 路径输出部分 ---
    int ex = 0, ey = 0; // 从起点 (0,0) 开始追踪
    // 只要没走到终点 (n-1, n-1)，就一直根据 path 数组往后找
    while (ex != n - 1 || ey != n - 1) {
        cout << ex << " " << ey << endl;
        
        int tmpx = ex, tmpy = ey;
        // 移动到下一个记录的坐标
        ex = path[tmpx][tmpy].first;
        ey = path[tmpx][tmpy].second;
    }
    // 最后输出终点坐标
    cout << n - 1 << " " << n - 1 << endl;
}

int main() {
    // 读入迷宫大小和地图
    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> g[i][j];
        }
    }
    
    bfs();
    
    return 0;
}
```

注意没走到终点是||不是&&!