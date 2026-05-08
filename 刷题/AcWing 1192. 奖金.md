---
tags:
  - graph
  - 拓扑排序
  - mid
date: 2026-03-22
练习次数: 3
---
# 题目

https://www.acwing.com/problem/content/1194/

由于无敌的凡凡在2005年世界英俊帅气男总决选中胜出，Yali Company总经理Mr.Z心情好，决定给每位员工发奖金。

公司决定以每个人本年在公司的贡献为标准来计算他们得到奖金的多少。

于是Mr.Z下令召开 m 方会谈。

每位参加会谈的代表提出了自己的意见：“我认为员工 a 的奖金应该比 b 高！”

Mr.Z决定要找出一种奖金方案，满足各位代表的意见，且同时使得总奖金数最少。

每位员工奖金最少为100元，且必须是整数。

#### 输入格式

第一行包含整数 n,m，分别表示公司内员工数以及参会代表数。

接下来 m 行，每行 2 个整数 a,b，表示某个代表认为第 a 号员工奖金应该比第 b 号员工高。

#### 输出格式

若无法找到合理方案，则输出“Poor Xed”；

否则输出一个数表示最少总奖金。

#### 数据范围

1≤n≤10000,  
1≤m≤20000,  
1≤a,b≤n

#### 输入样例：

```
2 1
1 2
```

#### 输出样例：

```
201
```

# 思路

参考[[AcWing 848. 有向图的拓扑序列]] [[AcWing 1169. 糖果]]
这里按照拓扑排序分配奖金,起点是工资低的员工,终点是工资高的员工
先判断是否有合理方案,即拓扑排序是否有解
然后从起点开始算终点的工资(可能有多个起点),一个终点可能有多个起点,所以每次取最大值

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

const int N=10010,M=20010;

// 邻接表存图
int n,m;
int h[N],e[M],ne[M],idx;

// d[i] 表示节点 i 的入度
int d[N];

// salary[i] 表示第 i 个人的工资
int salary[N];

// 拓扑排序使用的队列
queue<int> q;

// 存储拓扑序
vector<int> res;


// 向邻接表中加入一条边 a -> b
void add(int a,int b){
    e[idx]=b;       // 边指向 b
    ne[idx]=h[a];   // 新边的 next 指向原来的第一条边
    h[a]=idx++;     // 更新头节点
}


// 拓扑排序
void topsort(){

    // 将所有入度为0的点加入队列
    for(int i=1;i<=n;i++){
        if(d[i] == 0){
            q.push(i);
            res.push_back(i);  // 同时加入拓扑序列
        }
    }
    
    // BFS 过程
    while(!q.empty()){
        int u=q.front();
        q.pop();
        
        // 遍历 u 的所有邻接点
        for(int i=h[u];i!=-1;i=ne[i]){
            int v=e[i];

            // 删除边 u->v
            d[v]--;

            // 如果入度变为0
            if(d[v]==0){
                q.push(v);
                res.push_back(v);
            }
        }
    }
    
}

int main(){

    cin>>n>>m;

    // 初始化邻接表
    memset(h,-1,sizeof h);

    // 初始工资为100
    for(int i=1;i<=n;i++)
        salary[i]=100;

    // 输入关系
    while(m--){
        int a,b;
        cin>>a>>b;

        // b -> a
        // 表示 a 的工资必须比 b 高
        add(b,a);

        // a 入度+1
        d[a]++;
    }

    // 进行拓扑排序
    topsort();
    
    // 如果拓扑序数量 < n
    // 说明存在环
    if(res.size()<n){
        cout<<"Poor Xed"<<endl;
        return 0;
    }
    else{

        // 按拓扑序更新工资
        for(int i=0;i<res.size();i++){

            int u=res[i];

            // 遍历 u 的所有后继
            for(int k=h[u];k!=-1;k=ne[k]){
                int v=e[k];

                // 工资传播
                // v 至少比 u 高 1
                salary[v]=max(salary[v],salary[u]+1);
            }
        }
    }
    
    // 计算总工资
    int sum=0;
    for(int i=1;i<=n;i++){
        sum+=salary[i];
    }

    cout<<sum<<endl;
}
```