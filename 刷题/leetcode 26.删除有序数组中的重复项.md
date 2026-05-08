---
tags:
  - 数组
  - 双指针
  - easy
date: 2026-05-04
练习次数: 1
---
# 题目

[26. 删除有序数组中的重复项 - 力扣（LeetCode）](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/?envType=study-plan-v2&envId=top-interview-150)

给你一个 **非严格递增排列** 的数组 `nums` ，请你 **[原地](http://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k`。去重后，返回唯一元素的数量 `k`。

`nums` 的前 `k` 个元素应包含 **排序后** 的唯一数字。下标 `k - 1` 之后的剩余元素可以忽略。

**判题标准:**

系统会用下面的代码来测试你的题解:

```
int[] nums = [...]; // 输入数组
int[] expectedNums = [...]; // 长度正确的期望答案

int k = removeDuplicates(nums); // 调用

assert k == expectedNums.length;
for (int i = 0; i < k; i++) {
    assert nums[i] == expectedNums[i];
}

如果所有断言都通过，那么您的题解将被 **通过**。
```


**示例 1：**

```
**输入：**nums = [1,1,2]
**输出：**2, nums = [1,2,_]
**解释：**函数应该返回新的长度 **`2`** ，并且原数组 _nums_ 的前两个元素被修改为 **`1`**, **`2`** `。`不需要考虑数组中超出新长度后面的元素。

**示例 2：**

**输入：**nums = [0,0,1,1,1,2,2,3,3,4]
**输出：**5, nums = [0,1,2,3,4,_,_,_,_,_]
**解释：**函数应该返回新的长度 **`5`** ， 并且原数组 _nums_ 的前五个元素被修改为 **`0`**, **`1`**, **`2`**, **`3`**, **`4`** 。不需要考虑数组中超出新长度后面的元素。
```

# 思路
练习：[[leetcode 27.移除元素]]

https://leetcode.cn/problems/remove-duplicates-from-sorted-array/solutions/728105/shan-chu-pai-xu-shu-zu-zhong-de-zhong-fu-tudo

方法一：双指针
这道题目的要求是：对给定的有序数组 nums 删除重复元素，在删除重复元素之后，每个元素只出现一次，并返回新的长度，上述操作必须通过原地修改数组的方法，使用 O(1) 的空间复杂度完成。

由于给定的数组 nums 是有序的，因此对于任意 i<j，如果 nums[i]=nums[j]，则对任意 i≤k≤j，必有 nums[i]=nums[k]=nums[j]，即相等的元素在数组中的下标一定是连续的。利用数组有序的特点，可以通过双指针的方法删除重复元素。

如果数组 nums 的长度为 0，则数组不包含任何元素，因此返回 0。

当数组 nums 的长度大于 0 时，数组中至少包含一个元素，在删除重复元素之后也至少剩下一个元素，因此 nums[0] 保持原状即可，从下标 1 开始删除重复元素。

定义两个指针 fast 和 slow 分别为快指针和慢指针，快指针表示遍历数组到达的下标位置，慢指针表示下一个不同元素要填入的下标位置，初始时两个指针都指向下标 1。

假设数组 nums 的长度为 n。将快指针 fast 依次遍历从 1 到 n−1 的每个位置，对于每个位置，如果 nums[fast]

=nums[fast−1]，说明 nums[fast] 和之前的元素都不同，因此将 nums[fast] 的值复制到 nums[slow]，然后将 slow 的值加 1，即指向下一个位置。

遍历结束之后，从 nums[0] 到 nums[slow−1] 的每个元素都不相同且包含原数组中的每个不同的元素，因此新的长度即为 slow，返回 slow 即可。


# 代码

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        // 如果数组为空，直接返回 0
        if (nums.empty()) return 0;

        // i 是慢指针，指向当前已经处理好的“无重复序列”的末尾
        // j 是快指针，用来扫描整个数组
        int i = 1, j = 1;

        // 从第二个元素开始遍历（下标为 1）
        for(; j < nums.size(); j++){
            // 如果快指针发现当前元素与前一个元素不同
            // 说明找到了一个新的唯一元素
            if(nums[j] != nums[j-1]){
                // 将这个新元素搬运到慢指针 i 的位置
                nums[i] = nums[j];
                // 慢指针后移，准备接收下一个唯一元素
                i++;
            }
        }

        // i 的最终值即为去重后唯一元素的个数
        return i;
    }
};
```