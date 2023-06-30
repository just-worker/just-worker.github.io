---
title: "Pow"
date: 2023-06-30T21:35:04+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [Pow(x, n)](https://leetcode.cn/problems/powx-n/)

实现 pow(x, n) ，即计算 x 的整数 n 次幂函数（即，xn ）。


## 题目解析

注意折半计算，主要是递归太深入，使用额外变量进行存储奇数增生的数值。

```rust


// time: O(logN)
// space: O(1)
// n 是 i32, 直接反向可能溢出
impl Solution {
    pub fn my_pow(x: f64, n: i32) -> f64 {
        if x == 0.0 || x == 1.0 || n == 1{
            return x;
        }
        if n == 0 {
            return 1f64;
        }
        let (mut x, mut n, mut res, rev) = (x, n, 1.0, n < 0);
        loop {
            if n == 0 {
                break;
            }
            if n & 1 == 1 {
                res *= x;
            }
            x *= x;
            n = n / 2;
        }
        return if rev {1.0 / res} else {res};
    }
}
```

