---
tags:
  - graph
  - bfs
  - flood_fill
  - mid
date: 2026-03-21
练习次数: 2
---
# 题目

https://www.acwing.com/problem/content/1108/

FGD小朋友特别喜欢爬山，在爬山的时候他就在研究山峰和山谷。

为了能够对旅程有一个安排，他想知道山峰和山谷的数量。

给定一个地图，为FGD想要旅行的区域，地图被分为 n×n 的网格，每个格子 (i,j) 的高度 w(i,j) 是给定的。

若两个格子有公共顶点，那么它们就是相邻的格子，如与 (i,j) 相邻的格子有(i−1,j−1),(i−1,j),(i−1,j+1),(i,j−1),(i,j+1),(i+1,j−1),(i+1,j),(i+1,j+1)。

我们定义一个格子的集合 S 为山峰（山谷）当且仅当：

1. S 的所有格子都有相同的高度。
2. S 的所有格子都连通。
3. 对于 s 属于 S，与 s 相邻的 s′ 不属于 S，都有 $w_s>w_s^′$（山峰），或者 $w_s<w_s^′$（山谷）。
4. 如果周围不存在相邻区域，则同时将其视为山峰和山谷。

你的任务是，对于给定的地图，求出山峰和山谷的数量，如果所有格子都有相同的高度，那么整个地图即是山峰，又是山谷。

#### 输入格式

第一行包含一个正整数 n，表示地图的大小。

接下来一个 n×n 的矩阵，表示地图上每个格子的高度 w。

#### 输出格式

共一行，包含两个整数，表示山峰和山谷的数量。

#### 数据范围

1≤n≤1000,  
0≤w≤$10^9$

#### 输入样例1：

```
5
8 8 8 7 7
7 7 8 8 7
7 7 7 7 7
7 8 8 7 8
7 8 8 8 8
```

#### 输出样例1：

```
2 1
```

#### 输入样例2：

```
5
5 7 8 3 1
5 5 7 6 6
6 6 6 2 8
5 7 2 5 8
7 1 0 1 7
```

#### 输出样例2：

```
3 3
```

#### 样例解释

样例1：

![[Pasted image 20260305113836.png]]

样例2：

![[Pasted image 20260305113843.png]]

# 思路

![[Pasted image 20260305115041.png]]
对于各个点，进行 bfs，搜索所有和当前点等高的点。

并记录在搜索过程中，记录周围是否有高点和低点：

- 在搜索过程中，判断周围点的情况：
- 如过和当前点等高，则加入 bfs 队列。
- 如果比当前点高，记录下来。
- 如果比当前点地，记录下来。

如果在搜索过程中**没有**出现过比当前点更**高**的点，则该点及等高点组成**山峰**。

如果在搜索过程中**没有**出现过比当前点更**低**的点，则该点及等高点组成**山谷**。

# 代码

```c++
#include <iostream>
#include <queue>
#define x first   // 宏定义，方便用t.x访问pair的第一个元素（行坐标）
#define y second  // 宏定义，方便用t.y访问pair的第二个元素（列坐标）
using namespace std;

typedef pair<int, int> PII;  // 定义坐标对类型，存储(x,y)

const int N = 1010;  // 定义最大地图尺寸
int n;               // 地图边长
int h[N][N];         // 存储每个格子的高度
bool st[N][N];       // 标记数组，记录每个格子是否已被访问

// BFS函数：从(sx,sy)开始，遍历所有高度相同的连通块
// 参数has_higher: 引用传递，记录连通块周围是否存在更高格子
// 参数has_lower:  引用传递，记录连通块周围是否存在更低格子
void bfs(int sx, int sy, bool &has_higher, bool &has_lower) {
    queue<PII> q;           // BFS队列
    q.push({sx, sy});       // 起点入队
    st[sx][sy] = true;      // 标记起点已访问（注意：原代码缺少这行，应在入队时标记）
    
    while (!q.empty()) {
        PII t = q.front();  // 取出队首
        q.pop();
        
        // 遍历当前格子的8邻域（包括对角线）
        for (int i = t.x - 1; i <= t.x + 1; i++) {
            for (int j = t.y - 1; j <= t.y + 1; j++) {
                // 跳过越界的格子
                if (i < 1 || i > n || j < 1 || j > n) continue;
                
                // 情况1：相邻格子高度不同
                if (h[i][j] != h[t.x][t.y]) {
                    if (h[i][j] > h[t.x][t.y]) 
                        has_higher = true;  // 发现更高格子
                    else 
                        has_lower = true;   // 发现更低格子
                }
                // 情况2：相邻格子高度相同且未访问
                else if (!st[i][j]) {
                    q.push({i, j});      // 入队继续扩展
                    st[i][j] = true;     // 标记已访问
                }
            }
        }
    }
}

int main() {
    cin >> n;  // 读入地图尺寸
    // 读入高度矩阵（1-based索引）
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            cin >> h[i][j];
    
    int peak = 0, valley = 0;  // 山峰和山谷计数器
    
    // 遍历所有格子
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            if (!st[i][j]) {  // 遇到未访问的连通块
                bool has_higher = false, has_lower = false;
                bfs(i, j, has_higher, has_lower);  // 遍历该连通块
                
                // 周围没有更高格子 → 是山峰
                if (!has_higher) peak++;
                // 周围没有更低格子 → 是山谷
                if (!has_lower) valley++;
            }
        }
    }
    
    cout << peak << ' ' << valley << endl;  // 输出结果
    return 0;
}
```

