---
tags:
  - 双指针
  - 数组
  - easy
  - 面试经典150
date: 2026-04-24
练习次数: 1
---
# 题目

[88. 合并两个有序数组 - 力扣（LeetCode）](https://leetcode.cn/problems/merge-sorted-array/description/?envType=study-plan-v2&envId=top-interview-150)

给你两个按 **非递减顺序** 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 **合并** nums2 到 nums1 中，使合并后的数组同样按 **非递减顺序** 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。

## 示例 1

输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3  
输出：[1,2,2,3,5,6]  
解释：需要合并 [1,2,3] 和 [2,5,6] 。合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。

## 示例 2

输入：nums1 = [1], m = 1, nums2 = [], n = 0  
输出：[1]  
解释：需要合并 [1] 和 [] 。合并结果是 [1] 。

## 示例 3

输入：nums1 = [0], m = 0, nums2 = [1], n = 1  
输出：[1]  
解释：需要合并的数组是 [] 和 [1] 。合并结果是 [1] 。注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。

# 思路

跟归并排序的合并操作一致

# 代码

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        // 因为 nums1 的前 m 个元素在合并过程中会被覆盖，
        // 所以先拷贝一份备份，用于读取原始数据
        vector<int> backup1 = nums1;

        // 三个指针：
        // i 指向 backup1（原始 nums1 的有效元素）
        // j 指向 nums2
        // k 指向 nums1 中要写入的位置
        int i = 0, j = 0, k = 0;

        // 同时遍历两个数组，每次取较小的元素放入 nums1
        while (i < m && j < n) {
            int a = backup1[i]; // 从备份中取 nums1 的当前元素
            int b = nums2[j];   // 取 nums2 的当前元素

            if (a <= b) {
                nums1[k++] = a; // 取 a，移动 i 和 k
                i++;
            } else {
                nums1[k++] = b; // 取 b，移动 j 和 k
                j++;
            }
        }

        // 如果 backup1 中还有剩余元素，直接复制到 nums1 尾部
        while (i < m) nums1[k++] = backup1[i++];

        // 如果 nums2 中还有剩余元素，直接复制到 nums1 尾部
        while (j < n) nums1[k++] = nums2[j++];
    }
};
```