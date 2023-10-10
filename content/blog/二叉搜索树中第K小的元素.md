---
title: "二叉搜索树中第K小的元素"
date: 2023-10-10T23:03:34+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [二叉搜索树中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)

给定一个二叉搜索树的根节点 root ，和一个整数 k ，请你设计一个算法查找其中第 k 个最小元素（从 1 开始计数）。

![](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

>- 输入：root = [3,1,4,null,2], k = 1
>- 输出：1

## 题目解析

```rust

use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    pub fn kth_smallest(root: Option<Rc<RefCell<TreeNode>>>, k: i32) -> i32 {
        let mut arr = vec![];
        Self::to_array(root, &mut arr);
        return arr[k as usize - 1];
    }

    fn to_array(root: Option<Rc<RefCell<TreeNode>>>, collect: &mut Vec<i32>) {
        if let Some(node) = root {
            Self::to_array(node.borrow_mut().left.take(), collect);
            collect.push(node.borrow().val);
            Self::to_array(node.borrow_mut().right.take(), collect);
        }
    }
}
```

