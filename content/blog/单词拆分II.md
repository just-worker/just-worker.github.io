---
title: "单词拆分II"
date: 2023-08-11T20:52:15+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [单词拆分 II](https://leetcode.cn/problems/word-break-ii/)


给定一个字符串 s 和一个字符串字典 wordDict ，在字符串 s 中增加空格来构建一个句子，使得句子中所有的单词都在词典中。以任意顺序 返回所有这些可能的句子。

注意：词典中的同一个单词可能在分段中被重复使用多次。

>- 输入:s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
>- 输出:["cats and dog","cat sand dog"]

## 题目解析

和跳跃一样，有些不一定能跳跃到终点，但是从终点肯定能够判断是否可以到达原点。

```rust
// time: O(N)
// space: O(N)
use std::collections::HashSet;
impl Solution {
    pub fn word_break(s: String, word_dict: Vec<String>) -> Vec<String> {
        let word_set = word_dict.into_iter().collect();
        let mut res = vec![];
        Self::dfs(s.len(), &mut vec![], &mut res, &s, &word_set);
        return res;
    }

    fn dfs(end: usize, collect: &mut Vec<String>, res: &mut Vec<String>, s: &str, word_set: &HashSet<String>) {
        if end == 0 {
            res.push(collect.join(" ").to_string());
            return;
        }
        for begin in (0..end).rev() {
            let sub = &s[begin..end];
            if word_set.contains(sub) {
                collect.insert(0, sub.to_string());
                Self::dfs(begin, collect, res, s, word_set);
                collect.remove(0);
            }
        }
    }
}
```