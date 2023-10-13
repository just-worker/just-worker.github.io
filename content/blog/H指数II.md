---
title: "H指数II"
date: 2023-10-13T23:51:20+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [H 指数 II](https://leetcode.cn/problems/h-index-ii/)

给你一个整数数组 citations ，其中 citations[i] 表示研究者的第 i 篇论文被引用的次数，citations 已经按照 升序排列 。计算并返回该研究者的 h 指数。

h 指数的定义：h 代表“高引用次数”（high citations），一名科研人员的 h 指数是指他（她）的 （n 篇论文中）总共有 h 篇论文分别被引用了至少 h 次。

请你设计并实现对数时间复杂度的算法解决此问题。


>- 输入：citations = [0,1,3,5,6]
>- 输出：3 


## 题目解析

二分：当前满足，向左分。

```rust
// time: O(logN)
// space: O(1)

impl Solution {
    pub fn h_index(citations: Vec<i32>) -> i32 {
        if citations.is_empty() || citations[citations.len() - 1] == 0 {
            return 0;
        }
        if citations.len() == 1 {
            return 1.min(citations[0]);
        }
        let len = citations.len() as i32;
        let (mut left, mut right) = (0, len - 1);
        while left < right {
            let mid = left + (right - left) / 2;
            if citations[mid as usize].min(len) >= len - mid {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return len - left;
    }
}
```

