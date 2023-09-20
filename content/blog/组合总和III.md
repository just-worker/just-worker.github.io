---
title: "组合总和III"
date: 2023-09-20T20:40:13+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [组合总和 III](https://leetcode.cn/problems/combination-sum-iii/)

找出所有相加之和为 n 的 k 个数的组合，且满足下列条件：

- 只使用数字1到9
- 每个数字 最多使用一次 

返回 所有可能的有效组合的列表 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

>- 输入: k = 3, n = 7
>- 输出: [[1,2,4]]
>- 解释:
>- 1 + 2 + 4 = 7
>- 没有其他符合的组合了。


## 题目解析

```rust

impl Solution {
    pub fn combination_sum3(count: i32, target: i32) -> Vec<Vec<i32>> {
        if count >= target || 9 * count < target {
            return vec![];
        }
        let mut res = vec![];
        Self::dfs(1, 0, &mut vec![], target, count as usize, &mut res);
        return res;
    }

    fn dfs(current: i32, sum: i32, collect: &mut Vec<i32>, target: i32, count: usize, res: &mut Vec<Vec<i32>>) {
        if current == 11 || sum > target || collect.len() > count {
            return;
        }
        if sum == target && collect.len() == count {
            res.push(collect.clone());
            return;
        }
        Self::dfs(current + 1, sum, collect, target, count, res);
        if sum + current <= target {
            collect.push(current);
            Self::dfs(current + 1, sum + current, collect, target, count, res);
            collect.pop();
        }
    }
}
```
