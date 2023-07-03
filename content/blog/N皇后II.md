---
title: "N皇后II"
date: 2023-07-03T22:58:35+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [N皇后II](https://leetcode.cn/problems/n-queens-ii/)

n 皇后问题 研究的是如何将 n 个皇后放置在 n × n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回 n 皇后问题 不同的解决方案的数量。

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

>- 输入：n = 4
>- 输出：2
>- 解释：如上图所示，4 皇后问题存在两个不同的解法。

## 题目解析

和前一篇一致，废话不多说

```rust

// time: O(N!)
// space: O(N)
impl Solution {
    pub fn total_n_queens(n: i32) -> i32 {
        let mut res = 0;
        let mut used = vec![vec![false; n as usize]; n as usize];
        Self::dfs(&mut res, &mut used, 0);
        return res;
    }

    fn dfs(res: &mut i32, used: &mut Vec<Vec<bool>>, level: usize) {
        let len = used.len();
        if level == len {
            *res += 1;
            return;
        }
        for i in 0..len {
            if Self::check(used, level, i) {
                used[level][i] = true;
                Self::dfs(res, used, level + 1);
                used[level][i] = false;
            }
        }
    }
    
    fn check(used: &Vec<Vec<bool>>, r: usize, c: usize) -> bool {
        let len = used.len();
        for line in 0..len {
            // 列
            if used[line][c] {
                return false;
            }
            let delta = if line > r {
                line - r
            } else {
                r - line
            };
            // 斜线
            if delta + c < len && used[line][delta + c] {
                return false;
            } 
            if delta <= c && used[line][c - delta] {
                return false;
            }
        }
        return true;
    }
}
```

