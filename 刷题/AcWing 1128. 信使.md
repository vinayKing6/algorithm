---
tags:
  - graph
  - 最短路
  - easy
date: 2026-03-06
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/1130/

战争时期，前线有 n 个哨所，每个哨所可能会与其他若干个哨所之间有通信联系。

信使负责在哨所之间传递信息，当然，这是要花费一定时间的（以天为单位）。

指挥部设在第一个哨所。

当指挥部下达一个命令后，指挥部就派出若干个信使向与指挥部相连的哨所送信。

当一个哨所接到信后，这个哨所内的信使们也以同样的方式向其他哨所送信。**信在一个哨所内停留的时间可以忽略不计**。

直至所有 n 个哨所全部接到命令后，送信才算成功。

因为准备充足，每个哨所内都安排了足够的信使（如果一个哨所与其他 k 个哨所有通信联系的话，这个哨所内至少会配备 k 个信使）。

现在总指挥请你编一个程序，计算出完成整个送信过程最短需要多少时间。

#### 输入格式

第 1 行有两个整数 n 和 m，中间用 1 个空格隔开，分别表示有 n 个哨所和 m 条通信线路。

第 2 至 m+1 行：每行三个整数 i、j、k，中间用 1 个空格隔开，表示第 i 个和第 j 个哨所之间存在 **双向** 通信线路，且这条线路要花费 k 天。

#### 输出格式

一个整数，表示完成整个送信过程的最短时间。

如果不是所有的哨所都能收到信，就输出-1。

#### 数据范围

1≤n≤100,  
1≤m≤200,  
1≤k≤1000

#### 输入样例：

```
4 4
1 2 4
2 3 7
2 4 1
3 4 6
```

#### 输出样例：

```
11
```

# 思路

同 [[AcWing 1129. 热浪]]
只不过增加了判断是不是连通图,最后结果是最大距离(有点像关键路径)

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

// 常量定义
const int N = 110;   // 最大节点数
const int M = 210;   // 最大边数（每条边在无向图中存两次）
int n, m;            // n表示节点数，m表示边数
int h[N], e[M*2], ne[M*2], idx, w[M*2]; // 邻接表存储图
int dist[N];         // dist[i]表示从节点1到节点i的最短距离
int st[N];           // st[i]标记节点i是否已经确定最短距离

typedef pair<int,int> PII; // 定义一个pair类型，用于优先队列

// 添加一条从节点a到节点b、权值为c的边
void add(int a,int b,int c){
    e[idx] = b;      // 记录边的终点
    ne[idx] = h[a];  // 将新边挂在a节点的链表头
    w[idx] = c;      // 记录边的权值
    h[a] = idx++;    // 更新a的头指针，并自增idx
}

// Dijkstra算法求从节点1到所有节点的最短路
void dij(){
    memset(dist, 0x3f, sizeof dist); // 初始化dist为一个大数（表示无穷大）
    dist[1] = 0;                     // 起点到自己距离为0

    // 小根堆（优先队列）存放待更新的节点，优先弹出距离最小的节点
    priority_queue<PII, vector<PII>, greater<PII>> q;
    q.push({0, 1}); // 起点入队，距离为0

    while(!q.empty()){
        PII u = q.top();
        q.pop();
        int k = u.second; // 当前节点
        int d = u.first;  // 当前距离

        if(st[k]) continue; // 已确定最短距离的节点跳过
        st[k] = 1;          // 标记节点k已确定最短距离

        // 遍历节点k的所有邻边
        for(int i = h[k]; i != -1; i = ne[i]){
            int j = e[i]; // 邻接节点
            if(dist[j] > d + w[i]){ // 如果可以通过k更新距离
                dist[j] = d + w[i]; // 更新最短距离
                q.push({dist[j], j}); // 将更新后的节点入队
            }
        }
    }

    // 求从节点1出发到其他节点的最长最短距离
    int res = 0;
    for(int i = 2; i <= n; i++){
        if(dist[i] == 0x3f3f3f3f){ // 如果有节点不可达
            cout << -1 << endl;
            return;
        }
        res = max(res, dist[i]); // 取最长距离
    }
    cout << res << endl; // 输出结果
}

int main(){
    cin >> n >> m;
    memset(h, -1, sizeof h); // 初始化邻接表头指针为-1
    while(m--){
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c); // 添加边a->b
        add(b, a, c); // 添加边b->a（无向图）
    }
    dij(); // 调用Dijkstra算法
}
```