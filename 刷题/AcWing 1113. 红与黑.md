---
tags:
  - graph
  - dfs
  - 连通性
  - easy
date: 2026-03-17
练习次数: 1
---
# 题目

https://www.acwing.com/problem/content/1115/

有一间长方形的房子，地上铺了红色、黑色两种颜色的正方形瓷砖。

你站在其中一块黑色的瓷砖上，只能向相邻（上下左右四个方向）的黑色瓷砖移动。

请写一个程序，计算你总共能够到达多少块黑色的瓷砖。

#### 输入格式

输入包括多个数据集合。

每个数据集合的第一行是两个整数 W 和 H，分别表示 x 方向和 y 方向瓷砖的数量。

在接下来的 H 行中，每行包括 W 个字符。每个字符表示一块瓷砖的颜色，规则如下

1）‘.’：黑色的瓷砖；  
2）‘#’：红色的瓷砖；  
3）‘@’：黑色的瓷砖，并且你站在这块瓷砖上。该字符在每个数据集合中唯一出现一次。

当在一行中读入的是两个零时，表示输入结束。

#### 输出格式

对每个数据集合，分别输出一行，显示你从初始位置出发能到达的瓷砖数(记数时包括初始位置的瓷砖)。

#### 数据范围

1≤W,H≤20

#### 输入样例：

```
6 9 
....#. 
.....# 
...... 
...... 
...... 
...... 
...... 
#@...# 
.#..#. 
0 0
```

#### 输出样例：

```
45
```

# 思路
同[[AcWing 1112. 迷宫]]

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;
const int N=21;
int h,w,st[N][N],cnt,sx,sy;
char g[N][N];
typedef pair<int,int> PII;
int dx[]={0,1,0,-1};
int dy[]={1,0,-1,0};

void bfs(){
    memset(st,0,sizeof st);
    cnt=1;
    queue<PII> q;
    q.push({sx,sy});
    st[sx][sy]=1;
    
    while(!q.empty()){
        PII u=q.front();
        q.pop();
        
        for(int i=0;i<4;i++){
            int xx=u.first+dx[i];
            int yy=u.second+dy[i];
            if(xx<0 || xx>=h || yy<0 || yy>=w) continue;
            if(st[xx][yy]) continue;
            if(g[xx][yy]!='.') continue;
            cnt++;
            st[xx][yy]=1;
            q.push({xx,yy});
        }
    }
}

int main(){
    while(cin>>w>>h,w || h){
        for(int i=0;i<h;i++){
            for(int j=0;j<w;j++){
                cin>>g[i][j];
                if(g[i][j] == '@') {
                    sx=i;
                    sy=j;
                }
            }
        }
        bfs();
        cout<<cnt<<endl;
    }
}
```