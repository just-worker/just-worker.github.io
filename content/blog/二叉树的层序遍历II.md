---
title: "二叉树的层序遍历II"
date: 2023-07-21T21:38:36+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [二叉树的层序遍历 II](https://leetcode.cn/problems/binary-tree-level-order-traversal-ii/)

给你二叉树的根节点 root ，返回其节点值 自底向上的层序遍历 。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

>- 输入：root = [3,9,20,null,null,15,7]
>- 输出：[[15,7],[9,20],[3]]

## 题目解析

反转即可

```rust


// time: O(N)
// space: O(N)
use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    pub fn level_order_bottom(root: Option<Rc<RefCell<TreeNode>>>) -> Vec<Vec<i32>> {
        let mut res = vec![];
        if root.is_none() {
            return res;
        }
        let mut line = vec![root];
        while !line.is_empty() {
            let mut next = vec![];
            let mut collect = vec![];
            for node in line {
                if let Some(node) = node {
                    collect.push(node.borrow().val);
                    next.push(node.borrow().left.clone());
                    next.push(node.borrow().right.clone());
                }
            }
            if !collect.is_empty() {
                res.push(collect);
            }
            line = next;
        }
        res.reverse();
        return res;
    }
}
```

