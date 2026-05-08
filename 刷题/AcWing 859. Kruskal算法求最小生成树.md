---
tags:
  - graph
  - 最小生成树
  - kruskal
  - easy
  - 并查集
date: 2026-03-04
练习次数: 1
---
# 题目

https://www.acwing.com/activity/content/problem/content/925/

给定一个 n 个点 m 条边的无向图，图中可能存在重边和自环，边权可能为负数。

求最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

给定一张边带权的无向图 G=(V,E)，其中 V 表示图中点的集合，E 表示图中边的集合，n=|V|，m=|E|。

由 V 中的全部 n 个顶点和 E 中 n−1 条边构成的无向连通子图被称为 G 的一棵生成树，其中边的权值之和最小的生成树被称为无向图 G 的最小生成树。

#### 输入格式

第一行包含两个整数 n 和 m。

接下来 m 行，每行包含三个整数 u,v,w，表示点 u 和点 v 之间存在一条权值为 w 的边。

#### 输出格式

共一行，若存在最小生成树，则输出一个整数，表示最小生成树的树边权重之和，如果最小生成树不存在则输出 `impossible`。

#### 数据范围

1≤n≤105,  
1≤m≤2∗$10^5$,  
图中涉及边的边权的绝对值均不超过 1000。

#### 输入样例：

```
4 5
1 2 1
1 3 2
1 4 3
2 3 2
3 4 4
```

#### 输出样例：

```
6
```

# 思路

对比: [[AcWing 858. Prim算法求最小生成树]] [[AcWing 850. Dijkstra求最短路 II]]

算法思路：

- 将所有边按照**权值**的大小进行升序排序，然后从小到大**一一**判断。
    
- 如果这个边与之前选择的所有边**不会组成回路**，就选择这条边分；反之，舍去。
    
- 直到具有 n 个顶点的连通网筛选出来 n-1 条边为止。
    
- 筛选出来的边和所有的顶点构成此连通网的最小生成树。
    

判断是否会产生回路的方法为：使用并查集。

- 在初始状态下给各个个顶点在不同的集合中。、
    
- 遍历过程的每条边，判断这两个顶点的是否在一个集合中。
    
- 如果边上的这两个顶点在一个集合中，说明两个顶点已经连通，这条边不要。如果不在一个集合中，则要这条边。
    

---

举个例子，下图一个连通网，克鲁斯卡尔算法查找图 1 对应的最小生成树，需要经历以下几个步骤：

![[Pasted image 20260304151627.png]]

- 将连通网中的所有边按照权值大小做升序排序：

![[Pasted image 20260304151633.png]]

- 从 B-D 边开始挑选，由于尚未选择任何边组成最小生成树，且 B-D 自身不会构成环路，所以 B-D 边可以组成最小生成树。

![[Pasted image 20260304151638.png]]

- D-T 边不会和已选 B-D 边构成环路，可以组成最小生成树：

![[Pasted image 20260304151644.png]]

- A-C 边不会和已选 B-D、D-T 边构成环路，可以组成最小生成树：

![[Pasted image 20260304151650.png]]

- C-D 边不会和已选 A-C、B-D、D-T 边构成环路，可以组成最小生成树：

![[Pasted image 20260304151655.png]]

- C-B 边会和已选 C-D、B-D 边构成环路，因此不能组成最小生成树：

![[Pasted image 20260304151702.png]]

- B-T 、A-B、S-A 三条边都会和已选 A-C、C-D、B-D、D-T 构成环路，都不能组成最小生成树。而 S-A 不会和已选边构成环路，可以组成最小生成树。

![[Pasted image 20260304151707.png]]

- 如图下图 所示，对于一个包含 6 个顶点的连通网，我们已经选择了 5 条边，这些边组成的生成树就是最小生成树。

![[Pasted image 20260304151714.png]]


# 代码

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;
const int N = 100010;
int p[N];//保存并查集

struct E{
    int a;
    int b;
    int w;
    bool operator < (const E& rhs){//通过边长进行排序
        return this->w < rhs.w;
    }

}edg[N * 2];
int res = 0;

int n, m;
int cnt = 0;
int find(int a){//并查集找祖宗
    if(p[a] != a) p[a] = find(p[a]);
    return p[a];
}
void klskr(){
    for(int i = 1; i <= m; i++)//依次尝试加入每条边
    {
        int pa = find(edg[i].a);// a 点所在的集合
        int pb = find(edg[i].b);// b 点所在的集合
        if(pa != pb){//如果 a b 不在一个集合中
            res += edg[i].w;//a b 之间这条边要
            p[pa] = pb;// 合并a b
            cnt ++; // 保留的边数量+1
        }
    }
}
int main()
{

    cin >> n >> m;
    for(int i = 1; i <= n; i++) p[i] = i;//初始化并查集
    for(int i = 1; i <= m; i++){//读入每条边
        int a, b , c;
        cin >> a >> b >>c;
        edg[i] = {a, b, c};
    }
    sort(edg + 1, edg + m + 1);//按边长排序
    klskr();
    //如果保留的边小于点数-1，则不能连通
    if(cnt < n - 1) {
        cout<< "impossible";
        return 0;
    }
    cout << res;
    return 0;
}

作者：Hasity
链接：https://www.acwing.com/solution/content/104383/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```