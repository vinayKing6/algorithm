---
tags:
  - 模拟
  - mid
date: 2026-03-14
练习次数: 1
---
# 题目

https://www.acwing.com/activity/content/problem/content/2248/

给定一个包含 N 个正整数的序列，请你将序列中的元素以非递增顺序填充到螺旋矩阵中。

从左上角的第一个元素开始填充，并按照顺时针方向旋转。

要求矩阵有 m 行 n 列，并且 m,n 满足：

- m×n=N
- m≥n
- m−n 尽可能小

#### 输入格式

第一行包含整数 N。

第二行包含 N 个整数。

#### 输出格式

输出填充好的 m×n 的矩阵。

数字之间用一个空格隔开，结尾不得有多余空格。

#### 数据范围

1≤N≤10000,  
1≤ 序列中元素 ≤10000

#### 输入样例：

```
12
37 76 20 98 76 42 53 95 60 81 58 93
```

#### 输出样例：

```
98 95 93
42 37 81
53 20 76
58 60 76
```

# 思路

定义一个方向r,往右走到尽头向下转,往下走到尽头就往左转.....

# 代码
```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 10010;
int n, a[N], g[N][N];
typedef pair<int,int> PII;

// 方向控制数组：右、下、左、上 (顺时针顺序)
int dx[] = {0, 1, 0, -1};
int dy[] = {1, 0, -1, 0};

int r, c, d, sx, sy; // r,c:行和列; d:当前方向索引; sx,sy:当前坐标

// 螺旋填数函数
void put(int num){
    g[sx][sy] = num; // 在当前坐标填入数字
    
    // 计算按当前方向移动后的下一个坐标
    int tx = sx + dx[d];
    int ty = sy + dy[d];
    
    int cnt = 1; // 用于防止死循环（虽然在合法范围内不会发生）
    
    // 检查碰撞条件：越界 或者 该位置已经填过数
    while(tx < 0 || tx >= r || ty < 0 || ty >= c || g[tx][ty]){
        d = (d + 1) % 4; // 顺时针改变方向 (0->1->2->3->0)
        tx = sx + dx[d]; // 重新计算新方向下的下一个坐标
        ty = sy + dy[d];
        
        cnt++;
        if(cnt == n) break; // 所有点填完时退出
    }
    
    // 更新当前坐标为下一个合法位置
    sx = tx;
    sy = ty;
}

int main(){
    cin >> n;
    for(int i = 1; i <= n; i++) cin >> a[i];

    // 第一步：寻找矩阵的行数 r 和列数 c
    // 题目要求：r * c = n，r >= c，且 r - c 最小
    for(int i = sqrt(n); i <= n; i++){
        for(int j = sqrt(n); j >= 1; j--){
            if(i * j == n){
                r = i;
                c = j;
            }
            if(r || c) break;
        }
        if(r || c) break;
    }
    
    // 第二步：将输入的 n 个数按从大到小排序
    sort(a + 1, a + n + 1, greater<int>());

    // 第三步：按螺旋顺序依次填入数字
    for(int i = 1; i <= n; i++)
        put(a[i]);

    // 第四步：输出结果矩阵
    for(int i = 0; i < r; i++){
        for(int j = 0; j < c; j++){
            if(j) cout << " "; // 控制空格输出
            cout << g[i][j];
        }
        cout << endl;
    }
    return 0;
}
```