---
title: "X的平方根"
date: 2023-07-09T22:51:07+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [x的平方根](https://leetcode.cn/problems/sqrtx/)

给你一个非负整数 x ，计算并返回 x 的 算术平方根 。

由于返回类型是整数，结果只保留 整数部分 ，小数部分将被 舍去 。

注意：不允许使用任何内置指数函数和算符，例如 pow(x, 0.5) 或者 x ** 0.5 。

>- 输入：x = 4
>- 输出：2

## 题目解析

二分搜索即可
```rust

// time: O(logN)
// space: O(1)
impl Solution {
    pub fn my_sqrt(x: i32) -> i32 {
        if x < 2 {
            return x;
        }
        let (mut left , mut right) = (1, std::cmp::min(x / 2 + 1, 46341));
        while left < right {
            let mid = left + (right - left) / 2;
            if mid == left {
                break;
            }
            let square = mid * mid;
            if square == x {
                return mid;
            }
            if square > x {
                right = mid;
            } else {
                left = mid;
            }
        }
        return left;
    }
}
```

