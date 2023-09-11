---
title: "打家劫舍II"
date: 2023-09-11T22:46:30+08:00
draft: false
tags: ["leetcode", "算法"]
featured: true
---

## [打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/)

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 围成一圈 ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 在不触动警报装置的情况下 ，今晚能够偷窃到的最高金额。

>- 输入：nums = [2,3,2]
>- 输出：3
>- 解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。


## 题目解析

```rust

// time: O(N)
// space: O(1)
impl Solution {
    pub fn rob(nums: Vec<i32>) -> i32 {
        if nums.len() == 1 {
            return nums[0];
        } else if nums.len() == 2 {
            return nums[0].max(nums[1]);
        } else {
            let (mut t, mut f1, mut s1, mut f2, mut s2) = (0, nums[0], nums[0].max(nums[1]), nums[1], nums[1].max(nums[2]));
            for idx in 2..nums.len() {
                if idx != nums.len() - 1 {
                    t = s1;
                    s1 = s1.max(f1 + nums[idx]);
                    f1 = t;
                }
                if idx != 2 {
                    t = s2;
                    s2 = s2.max(f2 + nums[idx]);
                    f2 = t;
                }
            }
            return s1.max(s2);
        }
    }
}
```
