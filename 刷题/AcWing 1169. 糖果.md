---
tags:
  - graph
  - 差分约束
  - hard
  - spfa
  - 虚拟源点
  - 最长路
date: 2026-03-21
练习次数: 2
---
# 题目

https://www.acwing.com/problem/content/1171/

幼儿园里有 N 个小朋友，老师现在想要给这些小朋友们分配糖果，要求每个小朋友都要分到糖果。

但是小朋友们也有嫉妒心，总是会提出一些要求，比如小明不希望小红分到的糖果比他的多，于是在分配糖果的时候， 老师需要满足小朋友们的 K 个要求。

幼儿园的糖果总是有限的，老师想知道他至少需要准备多少个糖果，才能使得每个小朋友都能够分到糖果，并且满足小朋友们所有的要求。

#### 输入格式

输入的第一行是两个整数 N,K。

接下来 K 行，表示分配糖果时需要满足的关系，每行 3 个数字 X,A,B。

- 如果 X=1．表示第 A 个小朋友分到的糖果必须和第 B 个小朋友分到的糖果一样多。
- 如果 X=2，表示第 A 个小朋友分到的糖果必须少于第 B 个小朋友分到的糖果。
- 如果 X=3，表示第 A 个小朋友分到的糖果必须不少于第 B 个小朋友分到的糖果。
- 如果 X=4，表示第 A 个小朋友分到的糖果必须多于第 B 个小朋友分到的糖果。
- 如果 X=5，表示第 A 个小朋友分到的糖果必须不多于第 B 个小朋友分到的糖果。

小朋友编号从 1 到 N。

#### 输出格式

输出一行，表示老师至少需要准备的糖果数，如果不能满足小朋友们的所有要求，就输出 −1。

#### 数据范围

1≤N≤$10^5$,  
1≤K≤$10^5$,  
1≤X≤5,  
1≤A,B≤N,  
输入数据完全随机。

#### 输入样例：

```
5 7
1 1 2
2 3 2
4 4 1
3 4 5
5 4 5
2 3 5
4 5 1
```

#### 输出样例：

```
11
```

# 思路

![[Pasted image 20260312134310.png]]
![[Pasted image 20260312134331.png]]
这里求最小值,所以用最长路
比如:$A\geq B+1$,$A\geq C+1$ ,$B\geq 1$ ,$C\geq B+1$,都要满足,所以$A\geq C+1\geq B+1+1$ , A最小值就是路径最长的那个值
因为求最长路,所以spfa可以判断正环
# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10, M = N * 3;

int n, m;

// 链式前向星存图
int h[N], e[M], ne[M], w[M], idx;

// dist[i]：从超级源点0到i的最长距离
int dist[N];

// st[i]：节点i是否在栈中
int st[N];

// cnt[i]：节点i被更新的次数，用来判断是否存在正环
int cnt[N];


// 加边：a -> b 权值 c
void add(int a,int b,int c){
    e[idx] = b;        // 边指向的点
    ne[idx] = h[a];    // 下一条边
    w[idx] = c;        // 权值
    h[a] = idx++;      // 更新头节点
}


// SPFA求最长路
bool spfa(){

    // 初始化距离为负无穷
    memset(dist,-0x3f,sizeof dist);

    dist[0] = 0;       // 超级源点

    st[0] = 1;

    stack<int> s;      // 使用栈
    s.push(0);

    while(!s.empty()){

        int u = s.top();
        s.pop();
        st[u] = 0;

        // 遍历u的所有邻边
        for(int i = h[u]; i != -1; i = ne[i]){

            int v = e[i];

            // 松弛操作（最长路）
            if(dist[v] < dist[u] + w[i]){

                dist[v] = dist[u] + w[i];

                // 记录更新次数
                cnt[v] = cnt[u] + 1;

                // 若超过n次，说明存在正环
                if(cnt[v] >= n + 1) return false;

                // 若v不在栈中，则入栈
                if(!st[v]){
                    s.push(v);
                    st[v] = 1;
                }
            }
        }
    }

    return true;
}


int main(){

    cin >> n >> m;

    memset(h,-1,sizeof h);

    // 读入约束
    while(m--){

        int x,a,b;
        cin >> x >> a >> b;

        if(x == 1){
            // xa = xb
            add(a,b,0);
            add(b,a,0);

        }else if(x == 2){
            // xb ≥ xa + 1
            add(a,b,1);

        }else if(x == 3){
            // xa ≥ xb
            add(b,a,0);

        }else if(x == 4){
            // xa ≥ xb + 1
            add(b,a,1);

        }else{
            // xb ≥ xa
            add(a,b,0);
        }
    }

    // 超级源点 0 -> i
    for(int i = 1; i <= n; i++)
        add(0,i,1);

    if(spfa()){

        long long res = 0;

        for(int i = 1; i <= n; i++)
            res += dist[i];

        cout << res << endl;

    }else{
        cout << -1 << endl;
    }
}
```