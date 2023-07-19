---
title: "不同的二叉搜索树II"
date: 2023-07-19T23:01:25+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [不同的二叉搜索树 II](https://leetcode.cn/problems/unique-binary-search-trees-ii/)

给你一个整数 n ，请你生成并返回所有由 n 个节点组成且节点值从 1 到 n 互不相同的不同 二叉搜索树 。可以按 任意顺序 返回答案。

![](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

>- 输入：n = 3
>- 输出：[[1,null,2,null,3],[1,null,3,2],[2,1,3],[3,1,null,null,2],[3,2,null,1]]

## 题目解析

递归生成

```rust

// time: O(N)
// space: O(N)
use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    pub fn generate_trees(n: i32) -> Vec<Option<Rc<RefCell<TreeNode>>>> {
        return Self::generate(1, n);
    }

    fn generate(begin: i32, end: i32) -> Vec<Option<Rc<RefCell<TreeNode>>>> {
        if begin > end {
            return vec![None];
        }
        let mut res = vec![];
        for i in begin..=end {
            let left = Self::generate(begin, i - 1);
            let right = Self::generate(i + 1, end);
            
            for l in left.iter() {
                for r in right.iter() {
                    res.push(Some(Rc::new(RefCell::new(TreeNode {
                        val: i,
                        left: l.clone(),
                        right: r.clone()
                    }))));
                }
            }
        }
        return res;
    }
}
```

