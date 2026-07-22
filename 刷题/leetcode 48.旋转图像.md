---
tags:
  - 模拟
  - 数组
  - 矩阵
  - mid
date: 2026-07-22
练习次数: 2
---
# 题目

https://leetcode.cn/problems/rotate-image/description/

给定一个 _n_ × _n_ 的二维矩阵 `matrix` 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 **[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)** 旋转图像，这意味着你需要直接修改输入的二维矩阵。**请不要** 使用另一个矩阵来旋转图像。

![[Pasted image 20260506152720.png]]

**输入：** `matrix = [[1,2,3],[4,5,6],[7,8,9]]`
**输出：** `[[7,4,1],[8,5,2],[9,6,3]]`

# 思路

## 旋转图像：用翻转代替旋转

在处理矩阵顺时针旋转 $90^{\circ}$ 的问题时，除了逐个推导坐标变换公式，我们还可以另辟蹊径，通过两次翻转操作组合实现旋转。

### 1. 核心思路

以 $n \times n$ 矩阵为例，顺时针旋转 $90^{\circ}$ 可以拆解为以下两步：

1. **水平轴翻转**：将矩阵的第一行与最后一行交换，第二行与倒数第二行交换，以此类推。
    
2. **主对角线翻转**：将矩阵沿左上到右下的对角线进行对称交换（即行列坐标互换）。
    

### 2. 变换过程示例

以 $4 \times 4$ 矩阵为例：

**原始矩阵：**

$$\begin{bmatrix} 5 & 1 & 9 & 11 \\ 2 & 4 & 8 & 10 \\ 13 & 3 & 6 & 7 \\ 15 & 14 & 12 & 16 \end{bmatrix}$$

**第一步：水平轴翻转**

（第 $i$ 行与第 $n-i-1$ 行交换）

$$\xrightarrow{\text{水平翻转}} \begin{bmatrix} 15 & 14 & 12 & 16 \\ 13 & 3 & 6 & 7 \\ 2 & 4 & 8 & 10 \\ 5 & 1 & 9 & 11 \end{bmatrix}$$

**第二步：主对角线翻转**

（$matrix[row][col]$ 与 $matrix[col][row]$ 交换）

$$\xrightarrow{\text{主对角线翻转}} \begin{bmatrix} 15 & 13 & 2 & 5 \\ 14 & 3 & 4 & 1 \\ 12 & 6 & 8 & 9 \\ 16 & 7 & 10 & 11 \end{bmatrix}$$

此时得到的矩阵即为原始矩阵顺时针旋转 $90^{\circ}$ 后的结果。

---

### 3. 数学证明

我们可以通过坐标映射公式来验证这种方法的准确性：

- **水平轴翻转**的映射关系：
    
    $$matrix[row][col] \rightarrow matrix[n - row - 1][col]$$
    
- **主对角线翻转**的映射关系：
    
    $$matrix[row][col] \rightarrow matrix[col][row]$$
    

**联立变换：**

我们将第一步的结果代入第二步的映射中。假设经过水平翻转后的坐标为 $(row', col')$，其中 $row' = n - row - 1$，$col' = col$。

再进行主对角线翻转，坐标变为 $(col', row')$：

$$matrix[row][col] \xrightarrow{\text{最终结果}} matrix[col][n - row - 1]$$

这与顺时针旋转 $90^{\circ}$ 的通用公式 **$matrix_{new}[col][n - row - 1] = matrix[row][col]$** 完全一致。

---

# 代码实现 (C++)

C++

```c++
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    
    // 1. 水平轴翻转
    for (int i = 0; i < n / 2; ++i) {
        for (int j = 0; j < n; ++j) {
            swap(matrix[i][j], matrix[n - i - 1][j]);
        }
    }
    
    // 2. 主对角线翻转
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < i; ++j) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }
}
```

# 杂度分析

- **时间复杂度**：$O(N^2)$。其中 $N$ 是矩阵的行数。水平翻转需 $O(N^2/2)$，对角线翻转需 $O(N^2/2)$。
    
- **空间复杂度**：$O(1)$。原地旋转，不需要额外空间。