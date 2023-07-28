---
title: "单词接龙II"
date: 2023-07-28T22:09:27+08:00
draft: false
tags: ["leetcode", "算法"]
featured: true
---

## [单词接龙 II](https://leetcode.cn/problems/word-ladder-ii/)

按字典 wordList 完成从单词 beginWord 到单词 endWord 转化，一个表示此过程的 转换序列 是形式上像 beginWord -> s1 -> s2 -> ... -> sk 这样的单词序列，并满足：

- 每对相邻的单词之间仅有单个字母不同。
- 转换过程中的每个单词 si（1 <= i <= k）必须是字典 wordList 中的单词。注意，beginWord 不必是字典 wordList 中的单词。
- sk == endWord


给你两个单词 beginWord 和 endWord ，以及一个字典 wordList 。请你找出并返回所有从 beginWord 到 endWord 的 最短转换序列 ，如果不存在这样的转换序列，返回一个空列表。每个序列都应该以单词列表 [beginWord, s1, s2, ..., sk] 的形式返回。


>- 输入：beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
>- 输出：[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
>- 解释：存在 2 种最短的转换序列：
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"

## 题目解析

按照图来理解，其实就是最小路径算法，问题的核心是如何构建这个图。

和问题相关的性质有这几个

1. 终点限定，可能没有
2. 起点确定，字符序靠近
3. 只要最短路径，可能存在多个

这几个条件里面有部分是坑点

- 因为`字典序`的缘故，诞生一些顺序拼接字符再去匹配的解题办法，但是题目更优先的其实是`最短路径`，字符序是一个无用的题目限定。
- 最短路径，因为图是我们构建的，从头层级构建，按照广度遍历，触达终点立即停止，不会存在多种长度路径；图构建的时候就屏蔽多长度路径的情况。



因此，我们整体的步骤其实应该是

1. 构建图
2. 构建路径

顺道加上边界检测即可

```rust


impl Solution {
    pub fn find_ladders(
        begin_word: String,
        end_word: String,
        mut word_list: Vec<String>,
    ) -> Vec<Vec<String>> {
        // 没有终点
        if !word_list.contains(&end_word) {
            return vec![];
        }
        // 无需变换
        if begin_word == end_word {
            return vec![vec![begin_word]];
        }
        // 明确起始点和终点
        let (mut begin_word_idx, mut end_word_idx) = (None, None);
        for (idx, word) in word_list.iter().enumerate() {
            if word == &begin_word {
                begin_word_idx = Some(idx);
            } else if word == &end_word {
                end_word_idx = Some(idx);
            }
        }
        let end_word_idx = end_word_idx.unwrap();
        let begin_word_idx = match begin_word_idx {
            Some(idx) => idx,
            None => {
                word_list.push(begin_word);
                word_list.len() - 1
            }
        };
        // 构建连接关系 (distance, prev_list<idx>)
        let mut link: Vec<(i32, Vec<usize>)> = vec![(-1, vec![]); word_list.len()];
        // 起始位置
        link[begin_word_idx].0 = 0;
        for distance in 0.. {
            let mut find_next = false;
            for (from_idx, from) in word_list.iter().enumerate() {
                if link[from_idx].0 != distance {
                    continue;
                }
                for (to_idx, to) in word_list.iter().enumerate() {
                    if (link[to_idx].0 == -1 || link[to_idx].0 == distance + 1)
                        && Self::distance_1(from, to)
                    {
                        link[to_idx].0 = distance + 1;
                        link[to_idx].1.push(from_idx);
                        find_next = true;
                    }
                }
            }
            if !find_next || link[end_word_idx].0 != -1 {
                break;
            }
        }
        // 没有到达终点
        if link[end_word_idx].0 == -1 {
            return vec![];
        }
        return Self::build_path(end_word_idx, begin_word_idx, &word_list, &link);
    }

    fn distance_1(a: &str, b: &str) -> bool {
        a.bytes().zip(b.bytes()).fold(0, |distance, (c1, c2)| {
            distance + if c1 == c2 { 0 } else { 1 }
        }) == 1
    }

    fn build_path(
        to_idx: usize,
        from_idx: usize,
        word_list: &Vec<String>,
        link: &Vec<(i32, Vec<usize>)>,
    ) -> Vec<Vec<String>> {
        let to_word = &word_list[to_idx];
        // 查找的就是当前字符
        if to_idx == from_idx {
            return vec![vec![to_word.to_string()]];
        }
        let mut res = vec![];
        for &prev_idx in link[to_idx].1.iter() {
            let mut prev_path_list = Self::build_path(prev_idx, from_idx, word_list, link);
            // 反压
            prev_path_list
                .iter_mut()
                .for_each(|path| path.push(to_word.to_string()));
            res.append(&mut prev_path_list);
        }
        return res;
    }

    fn build_path_2(
        to_idx: usize,
        from_idx: usize,
        word_list: &Vec<String>,
        link: &Vec<(i32, Vec<usize>)>,
    ) -> Vec<Vec<String>> {
        let to_word = &word_list[to_idx];
        // 查找的就是当前字符
        if to_idx == from_idx {
            return vec![vec![to_word.to_string()]];
        }
        return link[to_idx]
            .1
            .iter()
            .flat_map(|&prev_idx| Self::build_path_2(prev_idx, from_idx, word_list, link))
            .map(|mut collect| {
                collect.push(to_word.to_string());
                collect
            })
            .collect();
    }
}
```

## 后续

从头构建或者从尾构建其实无所谓，因为都是广度遍历，主要是方向性问题；影响的是路径构建的方式。


可以看到，这里路径构建因为是从尾到头，所以采用的是反压的形式；如果从头构建路径的话，构建图的时候顺序就应该反过来。


