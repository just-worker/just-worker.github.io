---
title: "多数元素II"
date: 2023-10-10T22:51:45+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [多数元素 II](https://leetcode.cn/problems/majority-element-ii/)

给定一个大小为 n 的整数数组，找出其中所有出现超过 ⌊ n/3 ⌋ 次的元素。

>- 输入：nums = [3,2,3]
>- 输出：[3]


## 题目解析

计数

```rust


impl Solution {
    pub fn majority_element(nums: Vec<i32>) -> Vec<i32> {
        if nums.len() < 2 {
            return nums;
        }
        if nums.len() == 2 {
            if nums[0] == nums[1] {
                return vec![nums[0]];
            }
            return nums;
        }
        let count = nums.len() / 3;
        use std::collections::HashMap;
        let mut counter = nums.iter().fold(HashMap::new(), |mut c, v| {
            *c.entry(v).or_insert(0) += 1;
            c
        });
        nums.iter()
            .filter(|v| match counter.remove(v) {
                Some(c) if c > count => true,
                _ => false,
            })
            .map(|v| v.to_owned())
            .collect()
    }
}

```

