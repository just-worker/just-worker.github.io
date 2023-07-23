---
title: "路径总和II"
date: 2023-07-23T23:26:24+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [路径总和 II](https://leetcode.cn/problems/path-sum-ii/)

给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

![](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

>- 输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
>- 输出：[[5,4,11,2],[5,8,4,5]]

## 题目解析

```rust

// time: O(N)
// space: O(N)
use std::rc::Rc;
use std::cell::RefCell;
impl Solution {
    pub fn path_sum(root: Option<Rc<RefCell<TreeNode>>>, target_sum: i32) -> Vec<Vec<i32>> {
        let mut res = vec![];
        if root.is_none() {
            return res;
        }
        Self::dfs(&mut vec![], &mut res, root, target_sum);
        return res;
    }

    fn dfs(collect: &mut Vec<i32>, res: &mut Vec<Vec<i32>>, root: Option<Rc<RefCell<TreeNode>>>, target_sum: i32) {
        if root.is_none() {
            return;
        }
        let val = root.as_ref().unwrap().borrow().val;
        collect.push(val);
        let left = root.as_ref().unwrap().borrow().left.clone();
        let right = root.as_ref().unwrap().borrow().right.clone();
        if left.is_none() && right.is_none() && val == target_sum {
            res.push(collect.clone());
            collect.pop();
            return;
        }
        Self::dfs(collect, res, left, target_sum - val);
        Self::dfs(collect, res, right, target_sum - val);
        collect.pop();
    }
}
```

