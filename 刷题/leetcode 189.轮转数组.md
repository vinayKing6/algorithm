---
tags:
  - 数组
  - mid
  - 面试经典150
date: 2026-05-15
练习次数: 1
---
# 题目

[189. 轮转数组 - 力扣（LeetCode）](https://leetcode.cn/problems/rotate-array/description/?envType=study-plan-v2&envId=top-interview-150)

给定一个整数数组 `nums`，将数组中的元素向右轮转 `k` 个位置，其中 `k` 是非负数。

**示例 1:**

**输入:** `nums = [1,2,3,4,5,6,7], k = 3`
**输出:** `[5,6,7,1,2,3,4]`
**解释:**
向右轮转 1 步: `[7,1,2,3,4,5,6]`
向右轮转 2 步: `[6,7,1,2,3,4,5]`
向右轮转 3 步: `[5,6,7,1,2,3,4]`

**示例 2:**

**输入:** `nums = [-1,-100,3,99], k = 2`
**输出：**`[3,99,-1,-100]`
**解释:** 
向右轮转 1 步: `[99,-1,-100,3]`
向右轮转 2 步: `[3,99,-1,-100]`

# 思路

首先，可以copy一份，然后将copy数组往右移动k个位置存入原数组对应的位置

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        vector<int> copy=nums;
        for(int i=0;i<copy.size();i++){
            nums[(i+k)%nums.size()]=copy[i];
        }
    }
};
```

当然肯定不能这么简单，不然为什么是mid呢！

## 翻转数组

该方法基于如下的事实：当我们将数组的元素向右移动 k 次后，尾部 k mod n 个元素会移动至数组头部，其余元素向后移动 k mod n 个位置。

该方法为数组的翻转：我们可以先将所有元素翻转，这样尾部的 k mod n 个元素就被移至数组头部，然后我们再翻转 `[0,(k mod n) − 1] `区间的元素和 `[k mod n,n − 1]` 区间的元素即能得到最后的答案。

```python
nums = "----->-->"; k =3 
result = "-->----->"; 
reverse "----->-->" we can get "<--<-----" 
reverse "<--" we can get "--><-----" 
reverse "<-----" we can get "-->----->" 
this visualization help me figure it out :)
```

# 代码 (翻转数组版)

```c++
class Solution {
public:
    /**
     * 辅助函数：反转数组中指定范围 [s, e] 的元素
     * @param nums 引用传递原数组
     * @param s 起始索引 (start)
     * @param e 结束索引 (end)
     */
    void reverse(vector<int>& nums, int s, int e) {
        while (s < e) {
            swap(nums[s], nums[e]); // 交换首尾元素
            s++;                    // 向中间移动
            e--;
        }
    }

    /**
     * 主函数：将数组向右轮转 k 个位置
     */
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        // 1. 处理 k 大于数组长度的情况
        // 如果 k = n，数组不变；k > n 时，轮转 k 次等同于轮转 k % n 次
        k = k % n; 

        // 2. 三步反转法流程：
        // 第一步：反转整个数组
        // 结果：原数组末尾的元素被移到了前面，但顺序是反的
        reverse(nums, 0, n - 1);

        // 第二步：反转前 k 个元素
        // 结果：恢复前 k 个元素原本的相对顺序
        reverse(nums, 0, k - 1);

        // 第三步：反转剩余的 n-k 个元素
        // 结果：恢复后半部分元素的相对顺序
        reverse(nums, k, n - 1);
    }
};
```