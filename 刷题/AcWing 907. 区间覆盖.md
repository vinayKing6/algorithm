---
tags:
  - greedy
  - interval_scheduling
date: 2026-03-20
练习次数: 2
---

**思路:**

﻿ [**https://www.acwing.com/solution/content/16980/** ](https://www.acwing.com/solution/content/16980/)﻿

这是一个典型的**区间覆盖问题**的贪心算法实现。其核心目标是：给定一个目标区间  [start, end] 和一组可选区间，用最少的可选区间完全覆盖目标区间。

以下是该思路的简洁重写：

1. **预处理**：将所有可选区间**按左端点从小到大排序**。

2. **贪心选择**：遍历排序后的区间，在当前所有**左端点 ≤ start** 的区间中，选择**右端点最大**的一个。

3. **判断与推进**： 

- 如果找不到这样的区间，或者找到的最大右端点 **< start**，则说明无法覆盖，算法失败。

- 否则，将  start 更新为这个最大右端点，表示已覆盖到新的位置。

1. **循环与终止**：重复步骤2和3，直到  start ≥ end ，表示目标区间已被完全覆盖。

**算法核心**：每一步都选择能覆盖当前起点并延伸到最远的区间，以此达到使用最少区间数量的目的。

代码

```c++
#include<bits/stdc++.h>
using namespace std;
﻿

const int N=1e5+10;
int n;
﻿

// 定义区间结构体
struct Range{
  int l,r;  // 左端点和右端点
  
  // 重载小于运算符，按左端点从小到大排序
  bool operator < (const Range &w) const{
      return l<w.l;  // 按左端点排序，便于贪心选择
  }
};
 
int main(){
    Range ranges[N];  // 存储所有区间
    int st,ed;        // 目标覆盖区间的起点和终点
    int res=0;        // 最少区间数
    
    // 读入目标区间
    cin>>st>>ed;
    cin>>n;           // 区间数量
    
    // 读入所有区间
    for(int i=0;i<n;i++)
        cin>>ranges[i].l>>ranges[i].r;
    
    // 按左端点排序
    sort(ranges,ranges+n);
    
    bool success=0;   // 是否成功覆盖标记
    
    // 贪心算法主体
    for(int i=0;i<n;i++){
        // j从i开始遍历，r初始化为极小值(-2147483648)
        int j=i, r=0x80000000;
        
        // 找出所有左端点<=当前st的区间，并记录其中最大的右端点
        while(j<n && ranges[j].l<=st){
            r=max(r, ranges[j].r);
            j++;
        }
        
        // 如果找到的最大右端点都不能覆盖当前st，说明有间断，无法覆盖
        if(r < st)
            break;  // 退出循环，无解
            
        // 选择一个区间（能覆盖当前st且右端点最远）
        res++;
        st = r;     // 更新当前覆盖到的最右端
        
        // 检查是否已经覆盖到目标终点
        if(r >= ed){
            success = 1;
            break;  // 成功覆盖，退出
        }
        
        // 跳过已经检查过的区间，i会自增，所以这里设为j-1
        i = j-1;
    }
    
    // 输出结果
    if(success)
        cout<<res<<endl;
    else
        cout<<-1<<endl;  // 无法完全覆盖
        
    return 0;
}
﻿
```