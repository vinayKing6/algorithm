---
tags:
  - graph
  - 最短路
  - dijkstra
  - easy
date: 2026-03-06
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/1131/

德克萨斯纯朴的民众们这个夏天正在遭受巨大的热浪！！！

他们的德克萨斯长角牛吃起来不错，可是它们并不是很擅长生产富含奶油的乳制品。

农夫John此时身先士卒地承担起向德克萨斯运送大量的营养冰凉的牛奶的重任，以减轻德克萨斯人忍受酷暑的痛苦。

John已经研究过可以把牛奶从威斯康星运送到德克萨斯州的路线。

这些路线包括起始点和终点一共有 T 个城镇，为了方便标号为 1 到 T。

除了起点和终点外的每个城镇都由 **双向道路** 连向至少两个其它的城镇。

每条道路有一个通过费用（包括油费，过路费等等）。

给定一个地图，包含 C 条直接连接 2 个城镇的道路。

每条道路由道路的起点 Rs，终点 Re 和花费 Ci 组成。

求从起始的城镇 Ts 到终点的城镇 Te 最小的总费用。

#### 输入格式

第一行: 4 个由空格隔开的整数: T,C,Ts,Te;

第 2 到第 C+1 行: 第 i+1 行描述第 i 条道路，包含 3 个由空格隔开的整数: Rs,Re,Ci。

#### 输出格式

一个单独的整数表示从 Ts 到 Te 的最小总费用。

数据保证至少存在一条道路。

#### 数据范围

1≤T≤2500,  
1≤C≤6200,  
1≤Ts,Te,Rs,Re≤T,  
1≤Ci≤1000

#### 输入样例：

```
7 11 5 4
2 4 2
1 4 3
7 2 2
3 4 3
5 7 5
7 3 3
6 1 1
6 3 4
2 4 3
5 6 3
7 2 1
```

#### 输出样例：

```
7
```

# 思路

复习: [[AcWing 850. Dijkstra求最短路 II]]

![[Pasted image 20260306140554.png]]

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

// T: 最大点数, C: 最大边数
// 因为是无向图，每条边存两次，所以 e, w, ne 数组要开 2 倍 C
const int T = 2510, C = 6210;
int t, m, ts, te;
int h[T], e[C * 2], w[C * 2], ne[C * 2], idx; // 链式前向星存图
int dist[T];    // 存储起点到每个点的最短距离
int st[T];      // 标记该点的最短路径是否已确定

typedef pair<int, int> PII; // 用于优先队列：{距离, 节点编号}

// 链式前向星加边函数
void add(int a, int b, int c) {
    e[idx] = b;          // 存储终点
    w[idx] = c;          // 存储边权
    ne[idx] = h[a];      // 插入到表头
    h[a] = idx++;        // 更新表头
}

void dij() {
    // 1. 初始化距离为正无穷，并将起点距离设为 0
    memset(dist, 0x3f, sizeof dist);
    // 小根堆：优先弹出当前距离最短的点
    priority_queue<PII, vector<PII>, greater<PII>> q;
    
    dist[ts] = 0;
    q.push({0, ts});
    
    while (!q.empty()) {
        // 2. 取出当前距离起点最近的点
        PII u = q.top();
        q.pop();
        
        int d = u.first, k = u.second;
        
        // 3. 如果该点已处理过，跳过（Dijkstra 的贪心保证第一次弹出就是最短）
        if (st[k]) continue;
        st[k] = 1;
        
        // 4. 遍历当前点的所有邻居，进行松弛操作
        for (int i = h[k]; i != -1; i = ne[i]) {
            int j = e[i];
            // 如果通过当前点 k 到达 j 的路径更短，则更新并入队
            if (dist[j] > d + w[i]) {
                dist[j] = d + w[i];
                q.push({dist[j], j});
            }
        }
    }
    
    // 输出终点的最短距离
    cout << dist[te] << endl;
}

int main() {
    // 输入点数 t, 边数 m, 起点 ts, 终点 te
    if (!(cin >> t >> m >> ts >> te)) return 0;
    
    memset(h, -1, sizeof h); // 初始化链表头为 -1
    
    while (m--) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c); // 无向图需要添加两条有向边
        add(b, a, c);
    }
    
    dij();
    return 0;
}
```