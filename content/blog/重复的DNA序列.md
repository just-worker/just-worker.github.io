---
title: "重复的DNA序列"
date: 2023-08-26T23:03:51+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [重复的DNA序列](https://leetcode.cn/problems/repeated-dna-sequences/)

DNA序列 由一系列核苷酸组成，缩写为 'A', 'C', 'G' 和 'T'.。

- 例如，"ACGAATTCCG" 是一个 DNA序列 。

在研究 DNA 时，识别 DNA 中的重复序列非常有用。

给定一个表示 DNA序列 的字符串 s ，返回所有在 DNA 分子中出现不止一次的 长度为 10 的序列(子字符串)。你可以按 任意顺序 返回答案。

>- 输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
>- 输出：["AAAAACCCCC","CCCCCAAAAA"]


## 题目解析

移位计数

```rust
// time: O(N)
// space: O(N)
const LEN: usize = 10;
impl Solution {
    pub fn find_repeated_dna_sequences(s: String) -> Vec<String> {
        let len = s.len();
        if len <= LEN {
            return vec![];
        }
        use std::collections::HashMap;
        let mut counter = HashMap::new();
        for i in 0..=(len - LEN) {
            *counter.entry(&s[i..i + LEN]).or_insert(0) += 1;
        }
        return counter
            .iter()
            .filter(|&(_, &c)| c > 1)
            .map(|(&sub, _)| sub.to_string())
            .collect();
    }
}
```