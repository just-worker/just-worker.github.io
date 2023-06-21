---
title: "组合总和II"
date: 2023-06-21T22:24:39+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [组合总和II](https://leetcode.cn/problems/combination-sum-ii/)

给定一个候选人编号的集合 candidates 和一个目标数 target ，找出 candidates 中所有可以使数字和为 target 的组合。

candidates 中的每个数字在每个组合中只能使用 一次 。

注意：解集不能包含重复的组合。 

>- 输入: candidates = [10,1,2,7,6,1,5], target = 8,
>- 输出:
[
[1,1,6],
[1,2,5],
[1,7],
[2,6]
]

## 题目解析

经过前一题的洗礼，这道题并不困难，关键在于`不重复`，这个在之前的题目中也有出现过，我们只需要排序后加一些条件即可。

这里排序之后，还可以进行减枝：直接进行数字统计，在指定数字的时候直接遍历出同一个数字使用多次的情况。

```rust

impl Solution {
    pub fn combination_sum2(candidates: Vec<i32>, target: i32) -> Vec<Vec<i32>> {
        let counter = Self::sort_and_count(candidates);
        let mut res = vec![];
        Self::dfs(&mut vec![], &counter, &mut res, target, 0);
        return res;
    }

    fn dfs(collect: &mut Vec<i32>, counter: &Vec<(i32, usize)>, res: &mut Vec<Vec<i32>>, target: i32, n: usize) {
        let sum = collect.iter().sum::<i32>();
        if sum == target {
            res.push(collect.iter().map(|&v| v).collect());
            return;
        }
        if sum > target {
            return;
        }
        // 必须先判断 sum , 防止遗漏
        if n >= counter.len() {
            return;
        }
        // 跳过当前
        Self::dfs(collect, counter, res, target, n + 1);
        let current = counter[n];
        if sum + current.0 as i32 > target {
            return;
        }
        let times = std::cmp::min(current.1 as i32, (target - sum) / current.0 + 1);
        // 本次直接尝试使用多个数字
        for i in 1..=times {
            for _ in 0..i {
                collect.push(current.0);
            }
            Self::dfs(collect, counter, res, target, n + 1);
            for _ in 0..i {
                collect.pop();
            }
        }
    }

    // 计数
    fn sort_and_count(mut candidates: Vec<i32>) -> Vec<(i32, usize)>{
        candidates.sort();
        let mut res = vec![];
        for m in candidates {
            match res.last() {
                Some(&(v, c)) if v == m => {
                    res.pop();
                    res.push((m, c + 1));
                } ,
                _ => {
                    res.push((m, 1));
                }
            }
        }
        return res;
    }
}
```

