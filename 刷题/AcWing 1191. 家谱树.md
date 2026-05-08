---
tags:
  - graph
  - 拓扑排序
  - easy
date: 2026-03-06
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/1193/

有个人的家族很大，辈分关系很混乱，请你帮整理一下这种关系。

给出每个人的孩子的信息。

输出一个序列，使得每个人的孩子都比那个人后列出。

#### 输入格式

第 1 行一个整数 n，表示家族的人数；

接下来 n 行，第 i 行描述第 i 个人的孩子；

每行最后是 0 表示描述完毕。

每个人的编号从 1 到 n。

#### 输出格式

输出一个序列，使得每个人的孩子都比那个人后列出；

数据保证一定有解，如果有多解输出任意一解。

#### 数据范围

1≤n≤100

#### 输入样例：

```
5
0
4 5 1 0
1 0
5 3 0
3 0
```

#### 输出样例：

```
2 4 5 3 1
```

# 思路

参考: [[AcWing 848. 有向图的拓扑序列]]

# 代码

```c++
#include <bits/stdc++.h>
using namespace std;

const int N = 110;  // 最大节点数
int n;              // 节点数
int h[N], e[N*N], ne[N*N], idx; // 邻接表存储图
int d[N];           // 入度数组，d[i]表示节点i的入度
queue<int> q;       // 队列，用于BFS实现拓扑排序
vector<int> res;    // 存储拓扑排序结果

// 添加一条从节点a到节点b的有向边
void add(int a, int b){
    e[idx] = b;      // 存储边的终点
    ne[idx] = h[a];  // 将新边挂到a的邻接表链表头
    h[a] = idx++;    // 更新头指针并自增idx
}

// 拓扑排序函数
void topsort(){
    // 将入度为0的节点加入队列，同时加入结果数组
    for(int i = 1; i <= n; i++){
        if(d[i] == 0){
            q.push(i);
            res.push_back(i);
        }
    }

    // BFS处理队列
    while(!q.empty()){
        int u = q.front();
        q.pop();

        // 遍历节点u的所有邻接点
        for(int i = h[u]; i != -1; i = ne[i]){
            int j = e[i];  // 邻接点
            d[j]--;        // 邻接点入度减1
            if(d[j] == 0){ // 如果入度变为0，加入队列和结果数组
                q.push(j);
                res.push_back(j);
            }
        }
    }

    // 输出拓扑排序结果
    for(int i = 0; i < res.size() - 1; i++){
        cout << res[i] << " ";
    }
    cout << res[res.size() - 1] << endl;
}

int main(){
    cin >> n;                 // 输入节点数
    memset(h, -1, sizeof h);  // 初始化邻接表头指针为-1

    // 输入每个节点的出边
    for(int i = 1; i <= n; i++){
        int j;
        while(cin >> j, j != 0){ // 以0作为结束标记
            add(i, j);           // 添加有向边 i->j
            d[j]++;              // j的入度加1
        }
    }

    topsort(); // 执行拓扑排序
}
```