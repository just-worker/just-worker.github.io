---
title: "杨辉三角II"
date: 2023-07-25T23:17:09+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [杨辉三角 II](https://leetcode.cn/problems/pascals-triangle-ii/)

给定一个非负索引 rowIndex，返回「杨辉三角」的第 rowIndex 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

![](https://pic.leetcode-cn.com/1626927345-DZmfxB-PascalTriangleAnimated2.gif)

>- 输入: rowIndex = 3
>- 输出: [1,3,3,1]

## 题目解析

可以按照前一题的方式进行计算，但是有太多空间损耗。

其实每一行计算都是依赖上一行，并且数据多出1，从后计算就不会覆盖了。

```rust

// time: O(N ^ 2)
// space: O(N)
impl Solution {
    pub fn get_row(row_index: i32) -> Vec<i32> {
        let len = (row_index + 1) as usize;
        let mut res = vec![0; len];
        res[0] = 1;
        // 从后计算就不会覆盖
        for line in 1..len {
            for idx in (1..=line).rev() {
                res[idx] += res[idx - 1];
            }
        }
        return res;
    }
}
```

