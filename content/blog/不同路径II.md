---
title: "不同路径II"
date: 2023-07-07T21:08:16+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [ 不同路径II](https://leetcode.cn/problems/unique-paths-ii/)

一个机器人位于一个 m x n 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 1 和 0 来表示。

![](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

>- 输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
>- 输出：2
>- 解释：3x3 网格的正中间有一个障碍物。
>- 从左上角到右下角一共有 2 条不同的路径：
>- 1. 向右 -> 向右 -> 向下 -> 向下
>- 2. 向下 -> 向下 -> 向右 -> 向右

## 题目解析

老套路

```rust

// time: O(MN)
// space: O(MN)
impl Solution {
    pub fn unique_paths_with_obstacles(grid: Vec<Vec<i32>>) -> i32 {
        let (row, column) = (grid.len(), grid[0].len());
        let mut dp = vec![vec![0; column]; row];
        for r in 0..row {
            for c in 0..column {
                if grid[r][c] == 1 {
                    dp[r][c] == 0;
                    continue;
                }
                if r == 0 && c == 0 {
                    dp[r][c] = 1;
                    continue;
                }
                let left = if c > 0 { dp[r][c - 1] } else { 0 };
                let up = if r > 0 { dp[r - 1][c] } else { 0 };
                dp[r][c] = left + up;
            }
        }
        return dp[row - 1][column - 1];
    }
}

```

