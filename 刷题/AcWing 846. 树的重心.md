---
tags:
  - graph
  - dfs
  - mid
date: 2026-03-21
练习次数: 2
---
# 题目

https://www.acwing.com/problem/content/848/

给定一颗树，树中包含 n 个结点（编号 1∼n）和 n−1 条无向边。

请你找到树的重心，并输出将重心删除后，剩余各个连通块中点数的最大值。

重心定义：重心是指树中的一个结点，如果将这个点删除后，剩余各个连通块中点数的最大值最小，那么这个节点被称为树的重心。

#### 输入格式

第一行包含整数 n，表示树的结点数。

接下来 n−1 行，每行包含两个整数 a 和 b，表示点 a 和点 b 之间存在一条边。

#### 输出格式

输出一个整数 m，表示将重心删除后，剩余各个连通块中点数的最大值。

#### 数据范围

1≤n≤$10^5$

#### 输入样例

```
9
1 2
1 7
1 4
2 8
2 5
4 3
3 9
4 6
```

#### 输出样例：

```
4
```

# 思路

![[Pasted image 20260303130849.png]]
![[Pasted image 20260303130856.png]]
# 代码

```c++
#include <iostream>
#include <cstring>
using namespace std;

const int N=100010;
bool state[N];

//因为是双向边
int h[N],e[2*N],ne[2*N],idx,ans=N;
int n;
int add(int a,int b){
   e[idx]=b;
   ne[idx]=h[a];
   h[a]=idx++;
}

//返回的是以u为根的子树中点的数量
int dfs(int u){
    //标记u这个点被搜过
    state[u]=true;
    //size是表示将u点去除后，剩下的子树中数量的最大值；
    //sum表示以u为根的子树的点的多少，初值为1，因为已经有了u这个点
    int size=0,sum=1;
    for(int i=h[u];i!=-1;i=ne[i]){
        int j=e[i];
        if(state[j]) continue;
        //s是以j为根节点的子树中点的数量
        int s=dfs(j);
        //
        size=max(size,s);
        sum+=s;
    }
    //n-sum表示的是减掉u为根的子树，整个树剩下的点的数量
    size=max(size,n-sum);
    ans=min(size,ans);
    return sum;
}

int main(){
    cin>>n;
    memset(h,-1,sizeof h);
    int a,b;
    for(int i=0;i<n;i++){
        cin>>a>>b;
        add(a,b);
        add(b,a);
    }
    dfs(1);
    cout<<ans;
    return 0;
}

作者：acw_zxh
链接：https://www.acwing.com/solution/content/4917/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```
