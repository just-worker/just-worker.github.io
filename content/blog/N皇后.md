---
title: "N皇后"
date: 2023-07-03T22:48:44+08:00
draft: false
tags: ["leetcode", "算法"]
featured: true
---

## [N皇后](https://leetcode.cn/problems/n-queens/)

按照国际象棋的规则，皇后可以攻击与之处在同一行或同一列或同一斜线上的棋子。

n 皇后问题 研究的是如何将 n 个皇后放置在 n×n 的棋盘上，并且使皇后彼此之间不能相互攻击。

给你一个整数 n ，返回所有不同的 n 皇后问题 的解决方案。

每一种解法包含一个不同的 n 皇后问题 的棋子放置方案，该方案中 'Q' 和 '.' 分别代表了皇后和空位。

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

>- 输入：n = 4
>- 输出：[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]
>- 解释：如上图所示，4 皇后问题存在两个不同的解法。


## 爆破

爆破是必然的，但是怎么递归是个问题。

首先必须明确一点的是，爆破，其实就是不停地试数据，进一步的试数据，然后恢复现场试下一次数据。

因此，对于这种题目，我们可以总结如下步骤

1. 构建完成的答案结构，不保证内容正确
2. 构建数据合规检查，方便下一次试数据和提前返回
3. 填写本次数据，尝试下一轮试数，恢复现场
4. 试到最后一个说明符合条件，添加到答案


本次的问题点在于第四点，如何构建合规检查；这里先解释一下

- 每行不重复：每行只试一次，天然保证
- 每列不重复：遍历同列检查
- 斜线不重复：可以发现，行上面的偏移`offset`等于列`column`上面的偏移

因此，我们的解题步骤大概如下

1. 构建答案结构：这里为了方便使用二维布尔数组，方便过程检查，添加答案时候生成标准结构
2. 合规逻辑如上
3. 同规则
4. 生成答案

```rust

// time: O(N!)
// space: O(N)
impl Solution {
    pub fn solve_n_queens(n: i32) -> Vec<Vec<String>> {
        let mut res = vec![];
        let mut used = vec![vec![false; n as usize]; n as usize];
        Self::dfs(&mut res, &mut used, 0);
        return res;
    }

    fn dfs(res: &mut Vec<Vec<String>>, used: &mut Vec<Vec<bool>>, level: usize) {
        let len = used.len();
        if level == len {
            res.push(used.iter().map(|v|v.iter().map(|&b| if b {'Q'} else {'.'}).collect::<String>()).collect());
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

