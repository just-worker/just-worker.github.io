---
title: "摆动排序II"
date: 2023-11-14T23:01:31+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [摆动排序 II](https://leetcode.cn/problems/wiggle-sort-ii/)

给你一个整数数组 nums，将它重新排列成 nums[0] < nums[1] > nums[2] < nums[3]... 的顺序。

你可以假设所有输入数组都可以得到满足题目要求的结果。

>- 输入：nums = [1,5,1,1,6,4]
>- 输出：[1,6,1,5,1,4]

## 题目解析

排序之后大小分位即可，不过需要记住`田忌赛马`，否则最右边分界的时候会有点混乱。

```rust
impl Solution {
    pub fn wiggle_sort(nums: &mut Vec<i32>) {
        let mut sorted = nums.clone();
        sorted.sort_unstable();
        let (mut great_idx, mut less_idx) = (sorted.len() - 1, (sorted.len() + 1) / 2 - 1);
        for idx in (0..nums.len()).step_by(2) {
            nums[idx] = sorted[less_idx];
            less_idx -= 1;
            if idx + 1 < nums.len() {
                nums[idx + 1] = sorted[great_idx];
                great_idx -= 1;
            }
        }
    }
}
```

