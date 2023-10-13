---
title: "丑数II"
date: 2023-10-13T23:02:30+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [丑数 II](https://leetcode.cn/problems/ugly-number-ii/)

给你一个整数 n ，请你找出并返回第 n 个 丑数 。

丑数 就是只包含质因数 2、3 和/或 5 的正整数。

>- 输入：n = 10
>- 输出：12
>- 解释：[1, 2, 3, 4, 5, 6, 8, 9, 10, 12] 是由前 10 个丑数组成的序列。

## 题目解析

从小到大逐渐递增，注意重复情况也要自增。

```rust
// time: O(N)
// space: O(N)

impl Solution {
    pub fn nth_ugly_number(n: i32) -> i32 {
        if n == 1 {
            return 1;
        }
        let (n, mut dp) = (n as usize, vec![0, 1]);
        let (mut i2, mut i3, mut i5) = (1, 1, 1);
        for idx in 0..=n {
            let (r2, r3, r5) = (dp[i2] * 2, dp[i3] * 3, dp[i5] * 5);
            let r = r2.min(r3.min(r5));
            if r == r2 {
                i2 += 1;
            }
            if r == r3 {
                i3 += 1;
            }
            if r == r5 {
                i5 += 1;
            }
            dp.push(r);
        }
        return dp[n];
        
    }
}
```

