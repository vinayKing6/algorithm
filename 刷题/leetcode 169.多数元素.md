---
tags:
  - 数组
  - hashmap
  - 排序
  - easy
date: 2026-05-12
练习次数: 1
---
# 题目

[169. 多数元素 - 力扣（LeetCode）](https://leetcode.cn/problems/majority-element/description/?envType=study-plan-v2&envId=top-interview-150)

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

**示例 1：**

**输入** : nums = `[3,2,3]`
**输出** : `3`

**示例 2：**

**输入** : nums = `[2,2,1,1,1,2,2]`
**输出** : `2`

# 思路

很经典的题，用哈希表记录每个数字的出现次数就行，官方有5种题解，有兴趣的可以学学其他方法

# 代码

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        // 使用哈希表存储每个数字出现的次数
        // key: 数字本身, value: 出现的次数
        unordered_map<int, int> h;
        
        // 题目定义众数是指出现次数大于 n/2 的元素
        int cnt = nums.size() / 2;
        
        for (auto n : nums) {
            // 检查数字 n 是否已经在哈希表中
            // 注意：count() 返回 0 或 1。写成 == -1 逻辑上是不对的，
            // 实际上直接 h[n]++ 即可，因为不存在时默认初始化为 0。
            if (h.count(n) == 0) h[n] = 1; 
            else h[n]++;
            
            // 实时检查当前数字的出现次数
            // 一旦超过数组长度的一半，直接返回结果，提高效率
            if (h[n] > cnt) return n;
        }
        
        // 如果题目保证一定存在众数，这里其实走不到
        return 0;
    }
};
```