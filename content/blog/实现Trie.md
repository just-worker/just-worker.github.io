---
title: "实现Trie"
date: 2023-09-04T23:15:32+08:00
draft: false
tags: ["leetcode", "算法"]
featured: true
---

## [实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)

Trie（发音类似 "try"）或者说 前缀树 是一种树形数据结构，用于高效地存储和检索字符串数据集中的键。这一数据结构有相当多的应用情景，例如自动补完和拼写检查。

请你实现 Trie 类：

- Trie() 初始化前缀树对象。
- void insert(String word) 向前缀树中插入字符串 word 。
- boolean search(String word) 如果字符串 word 在前缀树中，返回 true（即，在检索之前已经插入）；否则，返回 false 。
- boolean startsWith(String prefix) 如果之前已经插入的字符串 word 的前缀之一为 prefix ，返回 true ；否则，返回 false 。


>- 输入
>- ["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
>- [[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]
>- 输出
>- [null, null, true, false, true, null, true]

>- 解释
>- Trie trie = new Trie();
>- trie.insert("apple");
>- trie.search("apple");   // 返回 True
>- trie.search("app");     // 返回 False
>- trie.startsWith("app"); // 返回 True
>- trie.insert("app");
>- trie.search("app");     // 返回 True


## 题目解析

```rust

#[derive(Default)]
struct Trie([Option<Box<Trie>>; 26], bool);

impl Trie {
    fn new() -> Self {
        Default::default()
    }

    fn insert(&mut self, word: String) {
        word.as_bytes()
            .iter()
            .map(|&c| (c - b'a') as usize)
            .fold(self, |trie, ch| {
                trie.0[ch].get_or_insert_with(Default::default)
            })
            .1 = true
    }

    fn exist(&self, word: String) -> (bool, bool) {
        let mut trie = self;
        for &ch in word.as_bytes().iter() {
            trie = match &trie.0[(ch - b'a') as usize] {
                Some(t) => t,
                None => return (false, false),
            }
        }
        return (true, trie.1);
    }

    fn search(&self, word: String) -> bool {
        return Self::exist(&self, word).1;
    }

    fn starts_with(&self, prefix: String) -> bool {
        return Self::exist(&self, prefix).0;
    }
}

```


