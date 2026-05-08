---
tags:
  - graph
  - 并查集
  - easy
date: 2026-03-21
练习次数: 2
---
# 题目

https://www.acwing.com/problem/content/description/839/

给定一个包含 n 个点（编号为 1∼n）的无向图，初始时图中没有边。

现在要进行 m 个操作，操作共有三种：

1. `C a b`，在点 a 和点 b 之间连一条边，a 和 b 可能相等；
2. `Q1 a b`，询问点 a 和点 b 是否在同一个连通块中，a 和 b 可能相等；
3. `Q2 a`，询问点 a 所在连通块中点的数量；

#### 输入格式

第一行输入整数 n 和 m。

接下来 m 行，每行包含一个操作指令，指令为 `C a b`，`Q1 a b` 或 `Q2 a` 中的一种。

#### 输出格式

对于每个询问指令 `Q1 a b`，如果 a 和 b 在同一个连通块中，则输出 `Yes`，否则输出 `No`。

对于每个询问指令 `Q2 a`，输出一个整数表示点 a 所在连通块中点的数量

每个结果占一行。

#### 数据范围

1≤n,m≤105

#### 输入样例：

```
5 5
C 1 2
Q1 1 2
Q2 1
C 2 5
Q2 5
```

#### 输出样例：

```
Yes
2
3
```

# 思路

复习[[AcWing 836. 合并集合]]

如何记录每个集合的数量?:
![[Pasted image 20260306155209.png]]
显然，将1,5合并  
find(1) = 3 find(5) = 4  
p[3] = 4  
这时候有8个点相连接  
合并的数目更新方式:  
size[3] = 4 以3为根节点下有4个连通块  
size[4] = 4 以4为根节点下有4个连通块  
更新4节点的连通块情况  
size[4] = size[4] + size[3] = 8

# 代码

```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 1e5 + 10;
int n, m;
int p[N]; // p[i] 表示节点 i 的父节点
int s[N]; // s[i] 表示以 i 为根的集合大小

// 并查集查找函数（带路径压缩）
int find(int x){
    if(p[x] != x){        // 如果x不是根节点
        p[x] = find(p[x]); // 递归查找根节点，并进行路径压缩
    }
    return p[x];          // 返回根节点
}

int main(){
    cin >> n >> m;

    // 初始化并查集
    for(int i = 1; i <= n; i++){
        p[i] = i; // 每个节点的父节点是自己
        s[i] = 1; // 每个集合初始大小为1
    }
    
    while(m--){ // 处理m次操作
        char op;
        cin >> op;

        // 合并操作 C a b
        if(op == 'C'){
            int a, b;
            cin >> a >> b;

            int pa = find(a); // 找到a的根
            int pb = find(b); // 找到b的根

            if(pa == pb) continue; // 如果已经在同一集合则跳过

            s[pb] += s[pa]; // 更新集合大小
            p[pa] = pb;     // 将a所在集合并入b所在集合
        }
        else{
            int op2;
            cin >> op2;

            // 查询是否在同一集合 Q 1 a b
            if(op2 == 1){
                int a, b;
                cin >> a >> b;

                int pa = find(a);
                int pb = find(b);

                if(pa == pb)
                    cout << "Yes" << endl;
                else
                    cout << "No" << endl;
            }
            else{ // 查询集合大小 Q 2 a
                int a;
                cin >> a;

                cout << s[find(a)] << endl; // 输出a所在集合大小
            }
        }
    }
}
```