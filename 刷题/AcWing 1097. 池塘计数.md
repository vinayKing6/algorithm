---
tags:
  - graph
  - bfs
  - flood_fill
  - easy
date: 2026-03-04
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/description/1099/

农夫约翰有一片 N∗M 的矩形土地。

最近，由于降雨的原因，部分土地被水淹没了。

现在用一个字符矩阵来表示他的土地。

每个单元格内，如果包含雨水，则用”W”表示，如果不含雨水，则用”.”表示。

现在，约翰想知道他的土地中形成了多少片池塘。

每组相连的积水单元格集合可以看作是一片池塘。

每个单元格视为与其上、下、左、右、左上、右上、左下、右下八个邻近单元格相连。

请你输出共有多少片池塘，即矩阵中共有多少片相连的”W”块。

#### 输入格式

第一行包含两个整数 N 和 M。

接下来 N 行，每行包含 M 个字符，字符为”W”或”.”，用以表示矩形土地的积水状况，字符之间没有空格。

#### 输出格式

输出一个整数，表示池塘数目。

#### 数据范围

1≤N,M≤1000

#### 输入样例：

```
10 12
W........WW.
.WWW.....WWW
....WW...WW.
.........WW.
.........W..
..W......W..
.W.W.....WW.
W.W.W.....W.
.W.W......W.
..W.......W.
```

#### 输出样例：

```
3
```

# 思路

复习: [[AcWing 844. 走迷宫]]

思路：

**bfs**

遍历单元格，同时用一个标记数组记录（标记）每个单元格是否被访问过。

在遍历单元格过程中，如果当前单元格是水，并且没有被访问过，水域数量+1，并且对该单元格进行bfs。

bfs 函数：从当前单元格开始，访问并标记和他同一片水域的单元格。

有点像连通块

# 代码

```c++
// c++
#include <cstring>
#include <iostream>
#include <algorithm>

#define x first
#define y second

using namespace std;

typedef pair<int, int> PII;

const int N = 1010, M = N * N;

int n, m;
char g[N][N];
PII q[M];
// 状态数组，标记点是否访问过
bool st[N][N];
//  Flood Fill 其实就是BFS
void bfs(int sx, int sy)
{
    // 队列保存能到达的点
    int hh = 0, tt = 0;
    // 队列中放入起点
    q[0] = {sx, sy};
    // 访问过的点状态置为True
    st[sx][sy] = true;
    // BFS
    while (hh <= tt)
    {
        // 取队头
        PII t = q[hh ++ ];
        // 遍历和队头相邻的点,计算相邻点坐标
        for (int i = t.x - 1; i <= t.x + 1; i ++ )
            for (int j = t.y - 1; j <= t.y + 1; j ++ )
            {
                // 不能是当前点
                if (i == t.x && j == t.y) continue;
                // 不能越界
                if (i < 0 || i >= n || j < 0 || j >= m) continue;
                // 该点不能时土地，也不能是访问过的点
                if (g[i][j] == '.' || st[i][j]) continue;
                // 排除了非法情况，该点访问状态置为1
                q[ ++ tt] = {i, j};
                // 该点加入队列
                st[i][j] = true;
            }
    }
}

int main()
{
    // 处理输入
    scanf("%d%d", &n, &m);
    for (int i = 0; i < n; i ++ ) scanf("%s", g[i]);
    // 记录答案的变量
    int cnt = 0;
    // 遍历各个点
    for (int i = 0; i < n; i ++ )
        for (int j = 0; j < m; j ++ )
            // 如果该点没有访问过，并且是水面
            if (g[i][j] == 'W' && !st[i][j])
            {
                // 从该点出发，执行Flood Fill算法
                bfs(i, j);
                // 池塘数量+1
                cnt ++ ;
            }

    printf("%d\n", cnt);

    return 0;
}

作者：Hasity
链接：https://www.acwing.com/solution/content/196675/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```