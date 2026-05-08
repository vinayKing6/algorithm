---
tags:
  - greedy
  - interval_scheduling
date: 2026-01-25
练习次数: 1
---

**思路：**

先按右端点给每一段区间排序，然后首先选择第一个区间的右端点，并遍历剩余的区间，若当前的区间的左端点小于等于选中的那个点，则继续循环（因为区间按右端点排序，所以当前区间右端点一定比选中的点大），若不然则更新选中的点为当前区间的右端点并重复。

代码：

```c++
#include<bits/stdc++.h>
using namespace std;

const int N = 1e5;  // 定义最大区间数量
int n, cnt, cur;    // n:区间数量, cnt:选择的点数, cur:当前选择的点的位置

// 定义区间结构体
struct Range {
    int l, r;  // 区间左右端点
    
    // 重载小于运算符，用于排序
    // 参数w: 要比较的另一个区间
    // 函数末尾const: 保证该函数不会修改当前对象
    bool operator < (const Range &w) const {
        // 按右端点从小到大排序（贪心策略）
        return r < w.r;
    }
};

int main() {
    Range ranges[N];  // 存储所有区间
    cin >> n;  // 输入区间数量
    
    // 读入所有区间
    for (int i = 0; i < n; i++) {
        // 注意：这里直接读取到结构体成员，不需要临时变量l,r
        cin >> ranges[i].l >> ranges[i].r;
    }
    
    // 按右端点升序排序
    sort(ranges, ranges + n);
    
    // 贪心算法：选择第一个区间的右端点作为第一个点
    cur = ranges[0].r;
    cnt = 1;  // 已选择1个点
    
    // 遍历剩余区间
    for (int i = 1; i < n; i++) {
        // 如果当前区间包含已选择的点(cur)，则跳过
        if (ranges[i].l <= cur)
            continue;
        else {
            // 否则，选择当前区间的右端点作为新点
            cnt++;  // 点数加1
            cur = ranges[i].r;  // 更新当前点位置
        }
    }
    
    cout << cnt << endl;  // 输出最少点数
    return 0;
}
﻿
```