---
tags:
  - graph
  - dijkstra
  - 最短路
  - easy
date: 2026-03-21
练习次数: 2
---
# 题目

https://www.acwing.com/activity/content/problem/content/1503/

有一天，琪琪想乘坐公交车去拜访她的一位朋友。

由于琪琪非常容易晕车，所以她想尽快到达朋友家。

现在给定你一张城市交通路线图，上面包含城市的公交站台以及公交线路的具体分布。

已知城市中共包含 n 个车站（编号1~n）以及 m 条公交线路。

每条公交线路都是 **单向的**，从一个车站出发直接到达另一个车站，两个车站之间可能存在多条公交线路。

琪琪的朋友住在 s 号车站附近。

琪琪可以在任何车站选择换乘其它公共汽车。

请找出琪琪到达她的朋友家（附近的公交车站）需要花费的最少时间。

#### 输入格式

输入包含多组测试数据。

每组测试数据第一行包含三个整数 n,m,s，分别表示车站数量，公交线路数量以及朋友家附近车站的编号。

接下来 m 行，每行包含三个整数 p,q,t，表示存在一条线路从车站 p 到达车站 q，用时为 t。

接下来一行，包含一个整数 w，表示琪琪家附近共有 w 个车站，她可以在这 w 个车站中选择一个车站作为始发站。

再一行，包含 w 个整数，表示琪琪家附近的 w 个车站的编号。

#### 输出格式

每个测试数据输出一个整数作为结果，表示所需花费的最少时间。

如果无法达到朋友家的车站，则输出 -1。

每个结果占一行。

#### 数据范围

n≤1000,m≤20000,  
1≤s≤n,  
0<w<n,  
0<t≤1000

#### 输入样例：

```
5 8 5
1 2 2
1 5 3
1 3 4
2 4 7
2 5 6
2 3 5
3 5 1
4 5 1
2
2 3
4 3 4
1 2 3
1 3 4
2 3 2
1
1
```

#### 输出样例：

```
1
-1
```

# 思路

模板题: [[AcWing 850. Dijkstra求最短路 II]]
进阶: [[AcWing 1135. 新年好]]

这里要反向建图,不然会TLE,反向建图只要一次dijkstra

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

const int N=1010,M=20010;

// n:节点数  m:边数
int n,m;

// 邻接表存图
int h[N],e[M],ne[M],idx;

// dist[i] 表示 i 到终点 k 的最短距离
int dist[N];

// k 表示目标点
int k;

// st[i] 表示该点是否已经确定最短路
int st[N];

// w[i] 表示第 i 条边的权值
int w[M];

// t 表示起点数量
int t;

// f 数组没有使用（可以删除）
int f[N];

// pair<距离,节点>
typedef pair<int,int> PII;


// 加边函数
// a -> b 权值为 c
void add(int a,int b,int c){
    e[idx]=b;       // 边指向 b
    ne[idx]=h[a];   // 指向原来的第一条边
    w[idx]=c;       // 边权
    h[a]=idx++;     // 更新头节点
}


// Dijkstra 求最短路
void dij(int s,int d[]){

    // 初始化距离为无穷
    for(int i=1;i<=n;i++)
        d[i] = 0x3f3f3f3f;

    // 初始化访问标记
    memset(st,0,sizeof st);

    // 小根堆（优先队列）
    priority_queue<PII,vector<PII>,greater<PII>> q;

    // 起点距离为0
    d[s]=0;

    // 入队
    q.push({0,s});
    
    while(!q.empty()){

        // 取出当前距离最小的点
        PII u=q.top();
        q.pop();

        int curV=u.second;  // 当前节点
        int curD=u.first;   // 当前距离
        
        // 如果已经确定最短路就跳过
        if(st[curV])
            continue;

        st[curV]=1;
        
        // 遍历所有邻边
        for(int i=h[curV];i!=-1;i=ne[i]){

            int j=e[i];  // 邻接点

            // 松弛操作
            if(d[j] > curD + w[i]){

                d[j] = curD + w[i];

                // 更新堆
                q.push({d[j],j});
            }
        }
    }
}


int main(){

    // 多组输入
    while(cin>>n){

        cin>>m>>k;

        // 初始化邻接表
        memset(h,-1,sizeof h);

        idx=0;

        // 输入 m 条边
        while(m--){

            int a,b,c;

            cin>>a>>b>>c;

            // 反向建图
            // 原图 a -> b
            // 改为 b -> a
            add(b,a,c);
        }

        // 从 k 开始跑一次 Dijkstra
        dij(k,dist);

        cin>>t;

        int res=0x3f3f3f3f;

        // t 个起点
        for(int i=1;i<=t;i++){

            int a;

            cin>>a;

            // 找最小距离
            res=min(res,dist[a]);
        }

        // 如果无法到达
        if(res == 0x3f3f3f3f)
            cout<<-1<<endl;
        else
            cout<<res<<endl;
    }
}
```