---
date: 2026-03-01
tags:
  - dp
  - 树形dp
  - easy
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/287/

Ural 大学有 N 名职员，编号为 1∼N。

他们的关系就像一棵以校长为根的树，父节点就是子节点的直接上司。

每个职员有一个快乐指数，用整数 $H_i$ 给出，其中 1≤i≤N。

现在要召开一场周年庆宴会，不过，没有职员愿意和直接上司一起参会。

在满足这个条件的前提下，主办方希望邀请一部分职员参会，使得所有参会职员的快乐指数总和最大，求这个最大值。

#### 输入格式

第一行一个整数 N。

接下来 N 行，第 i 行表示 i 号职员的快乐指数 $H_i$。

接下来 N−1 行，每行输入一对整数 L,K，表示 K 是 L 的直接上司。（注意一下，后一个数是前一个数的**父节点**，不要搞反）。

#### 输出格式

输出最大的快乐指数。

#### 数据范围

1≤N≤6000,  
−128≤$H_i$≤127

#### 输入样例：

```
7
1
1
1
1
1
1
1
1 3
2 3
6 4
7 4
4 5
3 5
```

#### 输出样例：

```
5
```

# 思路

状态转移方程：

-  `f[i][1]`表示选 i 号节点。
    
    > 那么很明显 i 号节点的快乐值需要算上  
    > 然后我们知道，如果选了这个点，它的所有字节点都是不能选的，所以我们应该给它加上 `f[j][0]`,j 表示子节点
    
-  `f[i][0]`表示不选 i 号节点。
    
    > 这时我们不需要加上 i 号点的快乐值  
    > 如果不选这个点，它的子节点可以选也可以不选，所以我们应该给它加上 `max f[j][0],f[j][1]`


![[Pasted image 20260301212630.png]]

# 代码

```c++
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int N = 6010;
int n;
int happy[N]; //每个职工的高兴度
int f[N][2]; //上面有解释哦~
int e[N],ne[N],h[N],idx; //链表，用来模拟建一个树
bool has_father[N]; //判断当前节点是否有父节点
void add(int a,int b){ //把a插入树中
    e[idx] = b,ne[idx] = h[a],h[a] = idx ++;
}
void dfs(int u){ //开始求解题目
    f[u][1] = happy[u]; //如果选当前节点u，就可以把f[u,1]先怼上他的高兴度
    for(int i = h[u];~i;i = ne[i]){ //遍历树
        int j = e[i];
        dfs(j); //回溯
        //状态转移部分，上面有详细讲解~
        f[u][0] += max(f[j][1],f[j][0]);
        f[u][1] += f[j][0];
    }
}
int main(){
    scanf("%d",&n);
    for(int i = 1;i <= n;i ++) scanf("%d",&happy[i]); //输入每个人的高兴度
    memset(h,-1,sizeof h); //把h都赋值为-1
    for(int i = 1;i < n;i ++){
        int a,b; //对应题目中的L,K,表示b是a的上司
        scanf("%d%d",&a,&b); //输入~
        has_father[a] = true; //说明a他有上司
        add(b,a); //把a加入到b的后面
    }
    int root = 1; //用来找根节点
    while(has_father[root]) root ++; //找根节点
    dfs(root); //从根节点开始搜索
    printf("%d\n",max(f[root][0],f[root][1])); //输出不选根节点与选根节点的最大值
    return 0;
}

作者：洛喑
链接：https://www.acwing.com/solution/content/105019/
来源：AcWing
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```