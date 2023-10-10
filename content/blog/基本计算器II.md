---
title: "基本计算器II"
date: 2023-10-09T23:25:32+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [基本计算器 II](https://leetcode.cn/problems/basic-calculator-ii/)

给你一个字符串表达式 s ，请你实现一个基本计算器来计算并返回它的值。

整数除法仅保留整数部分。

你可以假设给定的表达式总是有效的。所有中间结果将在 [-231, 231 - 1] 的范围内。

注意：不允许使用任何将字符串作为数学表达式计算的内置函数，比如 eval() 。


>- 输入：s = "3+2*2"
>- 输出：7

## 题目解析

```rust

impl Solution {
    pub fn calculate(s: String) -> i32 {
        let (mut value, mut values, mut op) = (0, vec![], '+');
        let chars = s.trim().chars().collect::<Vec<char>>();
        let len = chars.len();
        for (idx, ch) in  chars.into_iter().enumerate() {
            if ch == ' ' {
                continue;
            }
            let is_num = ch.is_digit(10);
            if is_num {
                value = value * 10 + (ch as u8 - b'0') as i32;
            } 
            if is_num && idx != len - 1 {
                continue;
            }
            match op {
                '+' => {
                    values.push(value);
                },
                '-' => {
                    values.push(-value);
                },
                '*' => {
                    *values.last_mut().unwrap() *= value;
                },
                '/' => {
                    *values.last_mut().unwrap() /= value;
                }
                _ => unreachable!()
            }
            value = 0;
            op = ch;
        }
        return values.iter().sum();
    }
}
```


