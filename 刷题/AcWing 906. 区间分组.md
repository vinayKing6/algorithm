---
tags:
  - greedy
  - interval_scheduling
  - mid
date: 2026-03-19
练习次数: 2
---
思路:

﻿ [**https://www.acwing.com/solution/content/14773/** ](https://www.acwing.com/solution/content/14773/)﻿

根据搜索结果，区间分组问题的核心贪心算法思路可概括如下：

1. **排序预处理**：将所有区间按左端点升序排列。

2. **维护组信息**：使用小根堆（优先队列）动态存储每个组当前的最大右端点值（即该组中所有区间右端点的最大值）。

3. **遍历决策**：依次处理每个区间： 

- 若堆为空 **或** 当前区间左端点 ≤ 堆顶（最小右端点），说明当前区间与所有已有组均相交，需**新建一组**，将其右端点入堆。

- 否则（当前区间左端点 > 堆顶），说明可**合并到堆顶对应组**，弹出堆顶后将该区间右端点入堆（更新该组最大右端点）。

1. **输出结果**：遍历结束后，堆的大小即为**最小组数**。

该算法本质是贪心选择：每次尽可能将区间放入右端点最小的组，若无法放入则必须开新组，从而保证组数最少。

代码:

```c++
// 包含所有常用标准库的头文件
#include<bits/stdc++.h>
using namespace std;
﻿

// 定义最大区间数量，1e5表示10的5次方，即100000
const int N=1e5+10;
int n; // 存储区间数量
﻿

// 定义区间结构体
struct Range{
  int l,r; // 区间的左端点和右端点
  
  // 重载小于运算符，用于按左端点升序排序
  bool operator < (const Range &w) const{
      return l<w.l; // 按左端点从小到大排序
  }
};
 
int main(){
    Range ranges[N]; // 定义一个区间数组
    
    cin>>n; // 输入区间数量
    
    // 循环读入n个区间的左右端点
    for(int i=0;i<n;i++ )
    {
        cin>>ranges[i].l>>ranges[i].r;
    }
    
    // 按区间的左端点进行升序排序
    sort(ranges,ranges+n);
    
    /*
     * 使用小根堆来维护所有组的右端点
     * greater<int> 表示小顶堆（最小元素在堆顶）
     * 堆中的每个元素代表一个组的当前最大右端点
     */
    priority_queue<int,vector<int>,greater<int>> h;
    
    // 遍历所有区间
    for(int i=0;i<n;i++){
        // 如果堆为空（还没有创建任何组）或者
        // 当前区间的左端点 <= 当前所有组中最小的右端点（即当前区间与所有组都有冲突）
        if(h.empty()||ranges[i].l<=h.top())
        {
            // 需要为当前区间创建一个新组
            h.push(ranges[i].r);
        }else{
            // 当前区间可以放入右端点最小的那个组中
            // 弹出最小右端点（更新该组的右端点）
            h.pop();
            // 插入当前区间的右端点作为该组的新右端点
            h.push(ranges[i].r);
        }
    }
    
    // 堆的大小就是最少需要的组数
    cout<<h.size()<<endl;
    
    return 0;
}
﻿
```