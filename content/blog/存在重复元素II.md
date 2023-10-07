---
title: "存在重复元素II"
date: 2023-10-07T23:09:44+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)

给你一个整数数组 nums 和一个整数 k ，判断数组中是否存在两个 不同的索引 i 和 j ，满足 nums[i] == nums[j] 且 abs(i - j) <= k 。如果存在，返回 true ；否则，返回 false 。

>- 输入：nums = [1,2,3,1], k = 3
>- 输出：true

## 题目解析

区间 -> 窗口
去重 -> 集合

```rust
// time: O(N)
// space: O(N)
impl Solution {
    pub fn contains_nearby_duplicate(nums: Vec<i32>, k: i32) -> bool {
        if nums.is_empty() || k == 0{
            return false;
        }
        let k = nums.len().min(k as usize);
        let mut set = std::collections::HashSet::new();
        for idx in 0..nums.len() {
            if idx < k {
                set.insert(nums[idx]);
            } else {
                if set.contains(&nums[idx]) {
                    return true;
                }
                set.insert(nums[idx]);
                set.remove(&nums[idx - k]);
            }
        }
        return set.len() < k;
    }
}
```

