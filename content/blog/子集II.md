---
title: "子集II"
date: 2023-07-18T23:05:39+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [子集 II](https://leetcode.cn/problems/subsets-ii/)

给你一个整数数组 nums ，其中可能包含重复元素，请你返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。返回的解集中，子集可以按 任意顺序 排列。

>- 输入：nums = [1,2,2]
>- 输出：[[],[1],[1,2],[1,2,2],[2],[2,2]]


## 题目解析

前面已经学过控制重复数值的办法，直接生成即可

```rust

impl Solution {
    pub fn subsets_with_dup(mut nums: Vec<i32>) -> Vec<Vec<i32>> {
        let len = nums.len();
        if len == 0 {
            return vec![vec![]];
        }
        if len == 1 {
            return vec![vec![], nums];
        }
        nums.sort();
        let mut res = vec![];
        let mut used = vec![false; len];
        Self::dfs(0, &mut vec![], &mut used, &nums, &mut res);
        return res;
    }

    fn dfs(i: usize, collect: &mut Vec<i32>, used: &mut Vec<bool>, nums: &Vec<i32>, res: &mut Vec<Vec<i32>>) {
        if i >= nums.len() {
            res.push(collect.iter().map(|&v| v).collect());
            return;
        }
        Self::dfs(i + 1, collect, used, nums, res);
        if !used[i] && (i < 1 || nums[i] != nums[i - 1] || used[i - 1]) {
            collect.push(nums[i]);
            used[i] = true;
            Self::dfs(i +  1, collect, used, nums, res);
            used[i] = false;
            collect.pop();
        }
    }
}
```
