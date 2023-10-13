---
title: "H指数"
date: 2023-10-13T23:37:08+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [H 指数](https://leetcode.cn/problems/h-index/)

给你一个整数数组 citations ，其中 citations[i] 表示研究者的第 i 篇论文被引用的次数。计算并返回该研究者的 h 指数。

根据维基百科上 h 指数的定义：h 代表“高引用次数” ，一名科研人员的 h 指数 是指他（她）至少发表了 h 篇论文，并且每篇论文 至少 被引用 h 次。如果 h 有多种可能的值，h 指数 是其中最大的那个。

>- 输入：citations = [3,0,6,1,5]
>- 输出：3

## 题目解析

只要记录大于`i`的有几篇文章即可，核心在于不用遍历低位，因为满足高位的必定满足低位，倒着数即可。

```rust
// time: O(N)
// space: O(N)
impl Solution {
    pub fn h_index(citations: Vec<i32>) -> i32 {
        if citations.is_empty() {
            return 0;
        }
        if citations.len() == 1 {
            return 1.min(citations[0]);
        }
        let len = citations.len();
        let counter = citations.into_iter().fold(vec![0; len + 1], |mut counter, v| {
            counter[len.min(v as usize)] += 1;
            counter
        });
        let mut c = 0;
        for i in (0..=len).rev() {
            c += counter[i];
            if c >= i as i32 {
                return i as i32;
            }
        }
        return 0;
    }
}
```

