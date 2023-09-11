---
title: "单词搜索II"
date: 2023-09-11T22:32:53+08:00
draft: false
tags: ["leetcode", "算法"]
featured: true
---

## [单词搜索 II](https://leetcode.cn/problems/word-search-ii/)

给定一个 m x n 二维字符网格 board 和一个单词（字符串）列表 words， 返回所有二维网格上的单词 。

单词必须按照字母顺序，通过 相邻的单元格 内的字母构成，其中“相邻”单元格是那些水平相邻或垂直相邻的单元格。同一个单元格内的字母在一个单词中不允许被重复使用。

![](https://assets.leetcode.com/uploads/2020/11/07/search1.jpg)

>- 输入：board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]
>- 输出：["eat","oath"]



## 题目解析

```rust


#[derive(Default)]
struct Trie([Option<Box<Trie>>; 26], bool);

impl Trie {
    fn new() -> Self {
        Default::default()
    }

    fn insert(&mut self, word: String)  {
        word.as_bytes().iter().fold(self, |trie, &ch| {
            trie.0[(ch - b'a') as usize].get_or_insert_with(Default::default)
        }).1 = true;
    }
}

impl From<Vec<String>> for Trie {
    fn from(value: Vec<String>) -> Self {
        let mut trie = Trie::new();
        value.into_iter().for_each(|s| trie.insert(s));
        return trie;
    }
}
impl Solution {
    pub fn find_words(board: Vec<Vec<char>>, words: Vec<String>) -> Vec<String> {
        let mut trie = words.into();
        let (row, column) = (board.len(), board[0].len());
        let (mut collect, mut res, mut visited) = (String::new(), vec![], vec![vec![false; column]; row]);
        for r in 0..row {
            for c in 0..column {
                Self::dfs(r, c, &mut collect, &mut res, &mut trie, &mut visited, &board);
            }
        }
        return res;
    }

    fn dfs(r: usize, c: usize, collect: &mut String, res: &mut Vec<String>, trie: &mut Trie, visited: &mut Vec<Vec<bool>>, board: &Vec<Vec<char>>) {
        let idx = (board[r][c] as u8 - b'a') as usize;
        let mut trie = match &mut trie.0[idx] {
            None => return,
            Some(t) => t.as_mut()
        };
        visited[r][c] = true;
        collect.push(board[r][c]);
        if trie.1 {
            res.push(collect.clone());
            trie.1 = false;
        }
        if r > 0 && !visited[r - 1][c] {
            Self::dfs(r - 1, c, collect, res, trie, visited, board);
        }
        if r + 1 < board.len() && !visited[r + 1][c] {
            Self::dfs(r + 1, c, collect, res, trie, visited, board);
        }
        if c > 0 && !visited[r][c - 1] {
            Self::dfs(r, c - 1, collect, res, trie, visited, board);
        }
        if c + 1 < board[0].len() && !visited[r][c + 1] {
            Self::dfs(r, c + 1, collect, res, trie, visited, board);
        }
        visited[r][c] = false;
        collect.pop();
    }
}
```


