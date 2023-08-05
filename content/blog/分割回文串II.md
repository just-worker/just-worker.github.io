---
title: "分割回文串II"
date: 2023-08-05T19:32:15+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [ 分割回文串 II](https://leetcode.cn/problems/palindrome-partitioning-ii/description/)

给你一个字符串 s，请你将 s 分割成一些子串，使每个子串都是回文。

返回符合要求的 最少分割次数 。

>- 输入：s = "aab"
>- 输出：1
>- 解释：只需一次分割就可将 s 分割成 ["aa","b"] 这样两个回文子串。


## 题目解析

和前一题一样，先做出回文DP，后续再利用一个DP得到最小跳跃。

```rust


// time: O(N ^ 2)
// space: O(N ^ 2)
impl Solution {
    pub fn min_cut(s: String) -> i32 {
        let len = s.len();
        if len < 2 {
            return 0;
        }
        // 回文DP
        let mut dp = vec![vec![false; len]; len];
        for begin in (0..len).rev() {
            dp[begin][begin] = true;
            for end in begin + 1 ..len {
                dp[begin][end] = &s[begin..=begin] == &s[end..=end] && (begin + 2 > end || dp[begin + 1][end - 1]);
            }
        }
        // 跳跃DP
        let mut dp2 = vec![len; len];
        for end in 0..len {
            // 一步跳
            if dp[0][end] {
                dp2[end] = 0;
            } else {
                for begin in 0..end {
                    if dp[begin + 1][end] {
                        // 如果前面不都是一步跳，肯定不会中
                        dp2[end] = std::cmp::min(dp2[end], dp2[begin] + 1);
                    }
                }
            }
        }
        return dp2[len - 1] as i32;
    }
}
```
