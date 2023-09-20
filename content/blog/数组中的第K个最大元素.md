---
title: "数组中的第K个最大元素"
date: 2023-09-20T20:35:52+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)

给定整数数组 nums 和整数 k，请返回数组中第 k 个最大的元素。

请注意，你需要找的是数组排序后的第 k 个最大的元素，而不是第 k 个不同的元素。

你必须设计并实现时间复杂度为 O(n) 的算法解决此问题。

>- 输入: [3,2,1,5,6,4], k = 2
>- 输出: 5

## 题目解析

```rust
impl Solution {
    pub fn find_kth_largest(nums: Vec<i32>, k: i32) -> i32 {
        // 默认实现的大顶堆
        use std::collections::BinaryHeap;
        let k = k as usize;
        let mut heap = BinaryHeap::with_capacity(k);
        for num in nums {
            if heap.len() < k {
                heap.push(-num);
            } else if -*heap.peek().unwrap() < num {
                heap.pop();
                heap.push(-num);
            }
        }
        return -heap.pop().unwrap();
    }
}
```
