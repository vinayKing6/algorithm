---
tags:
  - graph
  - 最短路
  - dijkstra
  - easy
date: 2026-03-06
练习次数: 2
---
# 题目

https://www.acwing.com/activity/content/problem/content/919/

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环，所有边权均为非负值。

请你求出 1 号点到 n 号点的最短距离，如果无法从 1 号点走到 n 号点，则输出 −1。

#### 输入格式

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

#### 输出格式

输出一个整数，表示 1 号点到 n 号点的最短距离。

如果路径不存在，则输出 −1。

#### 数据范围

1≤n,m≤$1.5×10^5$,  
图中涉及边长均不小于 0，且不超过 10000。  
数据保证：如果最短路存在，则最短路的长度不超过 $10^9$。

#### 输入样例：

```
3 3
1 2 2
2 3 1
1 3 4
```

#### 输出样例：

```
3
```

# 思路

![[Pasted image 20260303170137.png]]
![[Pasted image 20260303170145.png]]
注意：若要求任意点`i`到任意个点`j`的最短距离，只需修改`dijkstra`方法中的起源位置`dist[i] = 0`，以及返回为`dist[j]`

时间复杂度 O(mlogn)

每次找到最小距离的点沿着边更新其他的点，若`dist[j] > distance + w[i]`，表示可以更新`dist[j]`，更新后再把`j`点和对应的距离放入小根堆中。由于点的个数是`n`，边的个数是`m`，在极限情况下（稠密图$m=\frac{n(n−1)}{2}$）最多可以更新`m`回，每一回最多可以更新n个点（严格上是`n - 1`个点），有`m`回，因此最多可以把$n^2$个点放入到小根堆中，因此每一次更新小根堆排序的情况是O(log(n2))，一共最多`m`次更新，因此总的时间复杂度上限是O(mlog(($n^2$)))=O(2mlogn)=O(mlogn)

# 代码

```c++
#include<iostream>
#include<cstring>
#include<queue>

using namespace std;

typedef pair<int, int> PII;

const int N = 100010; // 把N改为150010就能ac

// 稀疏图用邻接表来存
int h[N], e[N], ne[N], idx;
int w[N]; // 用来存权重
int dist[N];
bool st[N]; // 如果为true说明这个点的最短路径已经确定

int n, m;

void add(int x, int y, int c)
{
    // 有重边也不要紧，假设1->2有权重为2和3的边，再遍历到点1的时候2号点的距离会更新两次放入堆中
    // 这样堆中会有很多冗余的点，但是在弹出的时候还是会弹出最小值2+x（x为之前确定的最短路径），
    // 并标记st为true，所以下一次弹出3+x会continue不会向下执行。
    w[idx] = c;
    e[idx] = y;
    ne[idx] = h[x]; 
    h[x] = idx++;
}

int dijkstra()
{
    memset(dist, 0x3f, sizeof(dist));
    dist[1] = 0;
    priority_queue<PII, vector<PII>, greater<PII>> heap; // 定义一个小根堆
    // 这里heap中为什么要存pair呢，首先小根堆是根据距离来排的，所以有一个变量要是距离，
    // 其次在从堆中拿出来的时候要知道知道这个点是哪个点，不然怎么更新邻接点呢？所以第二个变量要存点。
    heap.push({ 0, 1 }); // 这个顺序不能倒，pair排序时是先根据first，再根据second，
                         // 这里显然要根据距离排序
    while(heap.size())
    {
        PII k = heap.top(); // 取不在集合S中距离最短的点
        heap.pop();
        int ver = k.second, distance = k.first;

        if(st[ver]) continue;
        st[ver] = true;

        for(int i = h[ver]; i != -1; i = ne[i])
        {
            int j = e[i]; // i只是个下标，e中在存的是i这个下标对应的点。
            if(dist[j] > distance + w[i])
            {
                dist[j] = distance + w[i];
                heap.push({ dist[j], j });
            }
        }
    }
    if(dist[n] == 0x3f3f3f3f) return -1;
    else return dist[n];
}

int main()
{
    memset(h, -1, sizeof(h));
    scanf("%d%d", &n, &m);

    while (m--)
    {
        int x, y, c;
        scanf("%d%d%d", &x, &y, &c);
        add(x, y, c);
    }

    cout << dijkstra() << endl;

    return 0;
}

作者：optimjie
链接：https://www.acwing.com/solution/content/6554/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```