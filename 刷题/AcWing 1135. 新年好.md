---
tags:
  - graph
  - 最短路
  - dijkstra
  - mid
date: 2026-03-21
练习次数: 2
---
# 题目

https://www.acwing.com/problem/content/description/1137/

重庆城里有 n 个车站，m 条 **双向** 公路连接其中的某些车站。

每两个车站最多用一条公路连接，从任何一个车站出发都可以经过一条或者多条公路到达其他车站，但不同的路径需要花费的时间可能不同。

在一条路径上花费的时间等于路径上所有公路需要的时间之和。

佳佳的家在车站 1，他有五个亲戚，分别住在车站 a,b,c,d,e。

过年了，他需要从自己的家出发，拜访每个亲戚（顺序任意），给他们送去节日的祝福。

怎样走，才需要最少的时间？

#### 输入格式

第一行：包含两个整数 n,m，分别表示车站数目和公路数目。

第二行：包含五个整数 a,b,c,d,e，分别表示五个亲戚所在车站编号。

以下 m 行，每行三个整数 x,y,t，表示公路连接的两个车站编号和时间。

#### 输出格式

输出仅一行，包含一个整数 T，表示最少的总时间。

#### 数据范围

1≤n≤50000,  
1≤m≤$10^5$,  
1<a,b,c,d,e≤n,  
1≤x,y≤n,  
1≤t≤100

#### 输入样例：

```
6 6
2 3 4 5 6
1 2 8
2 3 3
3 4 4
4 5 5
5 6 2
1 6 7
```

#### 输出样例：

```
21
```

# 思路

思路：  
预处理，求六遍dijkstra，处理出`1,a,b,c,d,e`到图中各点的单源最短路。  
然后枚举所有从`1`出发的方案，比如`1->a->c->d->b->e`就是其中之一，将这些方案的路径和求个最小值即可（已经在预处理中解决了路径问题）。 而在这一步中，可以用dfs，也可以直接用STL中的`next_permutation`来解决。

代码出现的变量等数据解释：  
`id[x]` 表示下标为 xx 的点编号，我们规定`id[1]=1`，在样例中，`id[x]`恰好为`x`。  
`d[x][]` 表示从相应下标 xx 对应的点出发到其他点的单源最短路。  
`dijk(int st,int dist[])` `st`表示源点的`id`。

额...还是用dfs吧

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

// N为点数上限，M为边数上限（无向图需双倍存储，开至2e5+10）
const int N = 50010, M = 2e5 + 10;
int n, m, st[N], h[N], e[M], ne[M], idx, id[7], dist[7][N], w[M], ans = 0x3f3f3f3f, visited[6];
typedef pair<int,int> PII;

// 邻接表添加边：注意这里有个隐患，原代码add逻辑存的是 (b, a) 这种反向连接
void add(int a, int b, int c){
    w[idx] = c;
    e[idx] = b;      // e 存终点
    ne[idx] = h[a];  // ne 存下一条边索引
    h[a] = idx++;    // h 存头指针
}

// Dijkstra算法：计算单源最短路
void dij(int s, int d[]){
    for(int i = 1; i <= n; i++) d[i] = 0x3f3f3f3f;
    memset(st, 0, sizeof st); // st标记该点是否已求出最短路径
    priority_queue<PII, vector<PII>, greater<PII>> q;
    d[s] = 0;
    q.push({0, s});
    
    while(!q.empty()){
        PII u = q.top(); q.pop();
        int curV = u.second;
        
        if(st[curV]) continue;
        st[curV] = 1;
        
        for(int k = h[curV]; k != -1; k = ne[k]){
            int j = e[k];
            if(d[j] > d[curV] + w[k]){
                d[j] = d[curV] + w[k];
                q.push({d[j], j});
            }
        }
    }
}

// DFS：枚举访问5个亲戚点的全排列顺序
void dfs(int u, int sum, int pre){
    if(sum > ans) return; // 剪枝：当前路径已超过已知最优解
    if(u == 5){ // 5个点全部访问完毕
        ans = min(ans, sum); // 更新全局最优值
        return;
    }
    
    for(int i = 2; i <= 6; i++){
        if(visited[i]) continue;
        visited[i] = 1;
        // 计算从上一个点(pre)到当前点(id[i])的距离
        dfs(u + 1, sum + dist[pre][id[i]], i);
        visited[i] = 0; // 回溯
    }
}

int main(){
    cin >> n >> m;
    memset(h, -1, sizeof h);
    // id[2]~id[6] 存储需要访问的5个亲戚的编号
    for(int i = 2; i <= 6; i++) cin >> id[i];
    id[1] = 1; // 默认出发点为 1
    
    while(m--){
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
        add(b, a, c); // 无向图需添加双向边
    }
    
    // 计算关键点到所有点的距离
    // dist[1]存储出发点到各点距离，dist[i]存储各个亲戚点到各点距离
    dij(1, dist[1]); 
    for(int i = 2; i <= 6; i++) dij(id[i], dist[i]);
    
    // 从出发点 1 开始全排列搜索
    dfs(0, 0, 1);
    cout << ans << endl;
}
```