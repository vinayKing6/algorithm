---
tags:
  - graph
  - spfa
  - 负环
  - easy
  - 虚拟源点
date: 2026-03-21
练习次数: 2
---
# 题目

https://www.acwing.com/problem/content/854/

给定一个 n 个点 m 条边的有向图，图中可能存在重边和自环， **边权可能为负数**。

请你判断图中是否存在负权回路。

#### 输入格式

第一行包含整数 n 和 m。

接下来 m 行每行包含三个整数 x,y,z，表示存在一条从点 x 到点 y 的有向边，边长为 z。

#### 输出格式

如果图中**存在**负权回路，则输出 `Yes`，否则输出 `No`。

#### 数据范围

1≤n≤2000,  
1≤m≤10000,  
图中涉及边长绝对值均不超过 10000。

#### 输入样例：

```
3 3
1 2 -1
2 3 4
3 1 -4
```

#### 输出样例：

```
Yes
```

# 思路

### 算法分析

使用spfa算法解决是否存在负环问题

求负环的常用方法，基于`SPFA`，一般都用方法 `2`（该题也是用方法 `2`）：

- 方法 1：统计每个点入队的次数，如果某个点入队`n`次，则说明存在负环
- 方法 2：统计当前每个点的最短路中所包含的边数，如果某点的最短路所包含的边数大于等于`n`，则也说明存在环

**y总的原话**  
每次做一遍`spfa()`一定是正确的，但时间复杂度较高，可能会超时。初始时将所有点插入队列中可以按如下方式理解：  
在原图的基础上新建一个虚拟源点，从该点向其他所有点连一条权值为`0`的有向边。那么原图有负环等价于新图有负环。此时在新图上做`spfa`，将虚拟源点加入队列中。然后进行`spfa`的第一次迭代，这时会将所有点的距离更新并将所有点插入队列中。执行到这一步，就等价于视频中的做法了。那么视频中的做法可以找到负环，等价于这次`spfa`可以找到负环，等价于新图有负环，等价于原图有负环。得证。

- 1、`dist[x]` 记录虚拟源点到`x`的最短距离
    
- 2、`cnt[x]` 记录当前`x`点到**虚拟源点**最短路的边数，初始每个点到**虚拟源点**的距离为`0`，只要他能再走`n`步，即`cnt[x] >= n`，则表示该图中一定存在负环，由于从**虚拟源点**到`x`至少经过`n`条边时，则说明图中至少有`n + 1`个点，表示一定有点是重复使用
    
- 3、若`dist[j] > dist[t] + w[i]`,则表示从`t`点走到`j`点能够让权值变少，因此进行对该点`j`进行更新，并且对应`cnt[j] = cnt[t] + 1`,往前走一步
    

注意：该题是判断是否存在负环，并非判断是否存在从`1`开始的负环，因此需要将所有的点都加入队列中，更新周围的点  
![[Pasted image 20260304113751.png]]

#### 时间复杂度 一般：O(m) 最坏：O(nm)

# 代码

```c++
#include <cstring>
#include <iostream>
#include <queue>

using namespace std;

const int N = 2e3 + 10, M = 1e4 + 10;

int n, m;
int head[N], e[M], ne[M], w[M], idx;
bool st[N];
int dist[N];
int cnt[N]; //cnt[x] 表示 当前从1-x的最短路的边数

void add(int a, int b, int c)
{
    e[idx] = b;
    ne[idx] = head[a];
    w[idx] = c;
    head[a] = idx++;
}

bool spfa(){
    // 这里不需要初始化dist数组为 正无穷/初始化的原因是， 如果存在负环， 那么dist不管初始化为多少， 都会被更新

    queue<int> q;

    //不仅仅是1了， 因为点1可能到不了有负环的点， 因此把所有点都加入队列
    for(int i=1;i<=n;i++){
        q.push(i);
        st[i]=true;
    }

    while(q.size()){
        int t = q.front();
        q.pop();
        st[t]=false;
        for(int i = head[t];i!=-1; i=ne[i]){
            int j = e[i];
            if(dist[j]>dist[t]+w[i]){
                dist[j] = dist[t]+w[i];
                cnt[j] = cnt[t]+1;
                if(cnt[j]>=n){
                    return true;
                }
                if(!st[j]){
                    q.push(j);
                    st[j]=true;
                }
            }
        }
    }
    return false;
}

int main()
{
    cin >> n >> m;
    memset(head, -1, sizeof head);
    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        add(a, b, c);
    }

    if (spfa()) {
        cout << "Yes" << endl;
    }
    else {
        cout << "No" << endl;
    }
    return 0;
}

作者：Bug-Free
链接：https://www.acwing.com/solution/content/42308/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```