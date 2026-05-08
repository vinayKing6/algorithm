---
tags:
  - graph
  - bfs
date: 2026-03-04
练习次数: 2
---
# 题目

https://www.acwing.com/problem/content/description/846/

给定一个 $n×m$ 的二维整数数组，用来表示一个迷宫，数组中只包含 0 或 1，其中 0 表示可以走的路，1 表示不可通过的墙壁。

最初，有一个人位于左上角 (1,1) 处，已知该人每次可以向上、下、左、右任意一个方向移动一个位置。

请问，该人从左上角移动至右下角 (n,m) 处，至少需要移动多少次。

数据保证 (1,1) 处和 (n,m) 处的数字为 0，且一定至少存在一条通路。

输入格式

第一行包含两个整数 n 和 m。

接下来 n 行，每行包含 m 个整数（0 或 1），表示完整的二维数组迷宫。

输出格式

输出一个整数，表示从左上角移动至右下角的最少移动次数。

数据范围

1≤n,m≤100

输入样例：

```
5 5
0 1 0 0 0
0 1 0 1 0
0 0 0 0 0
0 1 1 1 0
0 0 0 1 0
```

输出样例：

```
8
```

# 思路

![[Pasted image 20260207172948.png]]

思路：从起点开始，往前走第一步，记录下所有第一步能走到的点，然后从所第一步能走到的点开始，往前走第二步，记录下所有第二步能走到的点，重复下去，直到走到终点。输出步数即可。

# 代码
```c++
// 包含所有标准库头文件（竞赛常用，但不推荐在生产代码中使用）
#include<bits/stdc++.h>
using namespace std;

// 定义迷宫最大尺寸为110x110（考虑到题目可能的边界）
const int N=110;

// 全局变量定义
int n, m; // n: 迷宫行数，m: 迷宫列数
int g[N][N]; // 存储迷宫地图，0表示可通行，1表示障碍
int path[N][N]; // 存储从起点到每个位置的最短步数，同时作为访问标记

// 定义坐标对类型，用于表示迷宫中的位置
typedef pair<int,int> PII;

// BFS函数，从起点(x,y)开始搜索到终点的最短路径
void bfs(int x,int y){
    queue<PII> q; // BFS使用的队列
    q.push({1,1}); // 起点入队
    
    // 四个方向的移动增量：右、左、下、上
    PII position[4]={{0,1},{0,-1},{1,0},{-1,0}};
    
    // BFS主循环：队列不为空时继续搜索
    while(!q.empty()){
        PII start = q.front(); // 取出队首位置
        q.pop(); // 弹出队首
        
        // 遍历四个方向
        for(int i=0;i<4;i++){
            // 计算新坐标
            int curX = start.first + position[i].first;
            int curY = start.second + position[i].second;
            
            // 检查新位置是否合法：
            // 1. 在迷宫范围内
            // 2. 未被访问过（path值为0）
            // 3. 是可通行区域（g值为0）
            if(curX <= n && curX >= 1 && curY <= m && curY >= 1 
               && path[curX][curY] == 0 && g[curX][curY] == 0){
                // 记录到新位置的最短步数 = 当前位置步数 + 1
                path[curX][curY] = path[start.first][start.second] + 1;
                // 新位置入队，继续搜索
                q.push({curX, curY});
            }
        }
    }
}

int main(){
    // 输入迷宫尺寸
    cin >> n >> m;
    
    // 读入迷宫地图
    for(int i=1; i<=n; i++)
        for(int j=1; j<=m; j++)
            cin >> g[i][j];
    
    // 从起点(1,1)开始BFS搜索
    bfs(1,1);
    
    // 输出从起点到终点(n,m)的最短步数
    cout << path[n][m] << endl;
    
    return 0;
}
```