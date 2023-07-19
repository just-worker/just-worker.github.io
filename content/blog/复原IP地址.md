---
title: "复原IP地址"
date: 2023-07-19T22:42:54+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)

有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

- 例如："0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的有效 IP 地址，这些地址可以通过在 s 中插入 '.' 来形成。你 不能 重新排序或删除 s 中的任何数字。你可以按 任何 顺序返回答案。

>- 输入：s = "25525511135"
>- 输出：["255.255.11.135","255.255.111.35"]

## 题目解析

- 如果是 0 ，独占
- 数值不能大于 255

```rust


impl Solution {
    pub fn restore_ip_addresses(s: String) -> Vec<String> {
        if s.len() == 0 {
            return vec![];
        }
        let mut res = vec![];
        Self::dfs(&mut res, &mut vec![], 0, s.as_str());
        return res;
    }

    fn dfs(res: &mut Vec<String>, collect: &mut Vec<String>, begin: usize, s: &str) {
        if collect.len() == 4 {
            if begin == s.len() {
                res.push(collect.join(".").to_string());
            }
            return;
        }

        for len in 1..4 {
            if begin + len > s.len() {
                break;
            }
            let slice = &s[begin..begin+len];
            let val = slice.parse::<i32>().unwrap();
            if slice.starts_with('0') && slice.len() > 1 || val > 255 {
                continue;
            } 
            collect.push(slice.to_string());
            Self::dfs(res, collect, begin + len, s);
            collect.pop();
        }
    }
}
```

