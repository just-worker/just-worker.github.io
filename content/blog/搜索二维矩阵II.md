---
title: "搜索二维矩阵II"
date: 2023-10-13T21:06:16+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/description/)

编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

![](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/searchgrid2.jpg)

>- 输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
>- 输出：true


## 题目解析

从数据的特征上看，我们可以得出一个结论：对于一个数据的左上角部分，一定小于当前数据；对于一个数据右下角部分，一定大于当前数据。

所以我们可以这样进行搜索
1. `matrix[r][0]`倒序搜索：如果数据小于当前值，可以直接分割右下角大部分
2. `matrix[_][c]`向右搜索：如果小于当前值，向右移动；因为左上角的数据一定小于当前数据
3. `matrix[r][_]`向下搜索：因为对于右下角更大，左上角较小，我们需要移动到右上角部分进行搜索

整体看来，其实就是从左下角移动到右上角的搜索过程，尽量的向上，不行就向右。

```rust

// time: O(M + N)
// space: O(1)
impl Solution {
    pub fn search_matrix(matrix: Vec<Vec<i32>>, target: i32) -> bool {
        if matrix[0][0] > target {
            return false;
        }
        let (row, column) = (matrix.len(), matrix[0].len());
        if matrix[row - 1][column - 1] < target {
            return false;
        }
        let (mut r, mut c) = (row - 1, 0);
        while c < column {
            if matrix[r][c] == target {
                return true;
            }
            if matrix[r][c] > target {
                if r == 0 {
                    return false;
                }
                r -= 1;
            } else {
                c += 1;
            }
        } 
        return false;
    }
}
```



