---
tags:
  - greedy
  - interval_scheduling
date: 2026-03-19
练习次数: 2
---

**思路：**

先按右端点给每一段区间排序，然后首先选择第一个区间的右端点，并遍历剩余的区间，若当前的区间的左端点小于等于选中的那个点，则继续循环（因为区间按右端点排序，所以当前区间右端点一定比选中的点大），若不然则更新选中的点为当前区间的右端点并且不相交区间数加一，重复以上操作。
一摸一样[[AcWing 905.区间选点]]

代码：

```c++
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+10;
int n;

// 定义区间结构体，包含左右端点
struct Range{
  int l,r;
  // 重载小于运算符，按右端点从小到大排序
  bool operator < (const Range &w) const{
      return r<w.r;
  }
};

int main(){
    Range ranges[N];
    cin>>n;
    for(int i=0;i<n;i++){
        cin>>ranges[i].l>>ranges[i].r;
    }
    
    // 按右端点从小到大排序
    sort(ranges,ranges+n);
    
    // 贪心算法：总是选择右端点最小的区间
    int ed=ranges[0].r;  // 记录当前已选区间的右端点
    int res=1;           // 已选择区间数量，第一个区间已选
    
    for(int i=1;i<n;i++){
        // 关键判断：如果当前区间左端点 <= 已选区间的右端点
        // 说明两个区间相交（包括端点重合的情况）
        // 注意：这里使用 <= 而不是 <，因为端点重合也算相交
        // 例如：区间[1,2]和[2,3]在端点2处重合，算相交
        // 对于单点区间[l,l]，如果l <= ed，说明该点包含在已选区间内
        if(ranges[i].l<=ed)
            continue;  // 跳过相交区间
        
        // 当前区间与已选区间不相交，选择该区间
        res++;
        ed=ranges[i].r;  // 更新已选区间的右端点
    }
    cout<<res<<endl;
}
﻿
```