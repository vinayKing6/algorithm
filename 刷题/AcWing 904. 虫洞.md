---
tags:
  - graph
  - spfa
  - 负环
  - hard
  - 虚拟源点
date: 2026-03-11
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/906/

农夫约翰在巡视他的众多农场时，发现了很多令人惊叹的虫洞。

虫洞非常奇特，它可以看作是一条 **单向** 路径，通过它可以使你回到过去的某个时刻（相对于你进入虫洞之前）。

农夫约翰的每个农场中包含 N 片田地，M 条路径（**双向**）以及 W 个虫洞。

现在农夫约翰希望能够从农场中的某片田地出发，经过一些路径和虫洞回到过去，并在他的出发时刻之前赶到他的出发地。

他希望能够看到出发之前的自己。

请你判断一下约翰能否做到这一点。

下面我们将给你提供约翰拥有的农场数量 F，以及每个农场的完整信息。

已知走过任何一条路径所花费的时间都不超过 10000 秒，任何虫洞将他带回的时间都不会超过 10000 秒。

#### 输入格式

第一行包含整数 F，表示约翰共有 F 个农场。

对于每个农场，第一行包含三个整数 N,M,W。

接下来 M 行，每行包含三个整数 S,E,T，表示田地 S 和 E 之间存在一条路径，经过这条路径所花的时间为 T。

再接下来 W 行，每行包含三个整数 S,E,T，表示存在一条从田地 S 走到田地 E 的虫洞，走过这条虫洞，可以回到 T 秒之前。

#### 输出格式

输出共 F 行，每行输出一个结果。

如果约翰能够在出发时刻之前回到出发地，则输出 `YES`，否则输出 `NO`。

#### 数据范围

1≤F≤5  
1≤N≤500,  
1≤M≤2500,  
1≤W≤200,  
1≤T≤10000,  
1≤S,E≤N

#### 输入样例：

```
2
3 3 1
1 2 2
1 3 4
2 3 1
3 1 3
3 2 1
1 2 3
2 3 4
3 1 8
```

#### 输出样例：

```
NO
YES
```

# 思路

复习:[[AcWing 852. spfa判断负环]]

吐槽一下,这个很考研阅读理解

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

// N 的大小取决于节点数，由于是邻接表存边，边数组 e, ne, w 建议开大一点（边数约为 2*M + W）
const int N = 5200; 

// h: 头结点数组, e: 存储终点, ne: 存储下一条边的索引, w: 存储边权, idx: 当前边索引
int F, n, dist[N], cnt[N], h[N], e[N], ne[N], idx, w[N], st[N];

// 标准邻接表加边函数
void add(int a, int b, int c) {
    e[idx] = b;      // 边到达的点
    w[idx] = c;      // 边的权重
    ne[idx] = h[a];  // 指向 a 节点的前一条边
    h[a] = idx++;    // 更新 a 节点的头结点
}

bool spfa() {
    // 每次进入 spfa 都要初始化
    memset(st, 0, sizeof st);      // 标记是否在队列中
    memset(dist, 0, sizeof dist);  // 最短距离（检测负环时初始为 0 即可）
    memset(cnt, 0, sizeof cnt);    // 记录到达该点经过的边数
    
    queue<int> q;
    
    // 将所有点入队，因为图中可能存在不连通的分量，负环可能在任何地方
    for(int i = 1; i <= n; i++) {
        q.push(i);
        st[i] = 1;
    }
    
    while(!q.empty()) {
        int u = q.front();
        q.pop();
        st[u] = 0; // 出队后重置标记
        
        // 遍历从 u 出发的所有边
        for(int i = h[u]; ~i; i = ne[i]) {
            int j = e[i];
            
            // 松弛操作：如果通过 u 到达 j 的路径更短
            if(dist[j] > dist[u] + w[i]) {
                dist[j] = dist[u] + w[i];
                cnt[j] = cnt[u] + 1; // 边数更新
                
                // 关键点：如果某个点经过了 n 条边，说明存在负环（抽屉原理）
                if(cnt[j] >= n) return true; 
                
                // 如果 j 不在队列中，则入队准备更新其后续节点
                if(!st[j]) {
                    st[j] = 1;
                    q.push(j);
                }
            }
        }
    }
    
    return false; // 队列为空且未发现负环
}

int main() {
    ios::sync_with_stdio(false); // 优化输入输出效率
    cin >> F; // 测试用例数量
    while(F--) {
        int m, w_count;
        cin >> n >> m >> w_count;
        
        memset(h, -1, sizeof h); // 初始化邻接表头
        idx = 0;
        
        // 读入 m 条普通的双向路径（权值为正）
        while(m--) {
            int a, b, c;
            cin >> a >> b >> c;
            add(a, b, c);
            add(b, a, c);
        }
        
        // 读入 w 条虫洞路径（权值为负，且为单向）
        while(w_count--) {
            int a, b, c;
            cin >> a >> b >> c;
            add(a, b, -c); // 虫洞使时间倒流，权值为负
        }
        
        // 如果检测到负环，说明可以回到过去
        if(spfa())
            cout << "YES" << endl;
        else
            cout << "NO" << endl;
    }
    return 0;
}
```