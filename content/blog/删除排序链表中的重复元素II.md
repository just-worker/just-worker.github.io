---
title: "删除排序链表中的重复元素II"
date: 2023-07-16T22:44:31+08:00
draft: false
tags: ["leetcode", "算法"]
---


## [删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/)

给定一个已排序的链表的头 head ， 删除原始链表中所有重复数字的节点，只留下不同的数字 。返回 已排序的链表 。

>- 输入：head = [1,2,3,3,4,4,5]
>- 输出：[1,2,5]

## 题目解析

### 计数

```rust

// time: O(N)
// space: O(N)
impl Solution {
    pub fn delete_duplicates(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        if head.is_none() || head.as_ref().unwrap().next.is_none() {
            return head;
        }
        use std::collections::HashMap;
        let mut counter = HashMap::new();
        let mut head = head;
        let mut cursor = &mut head;
        while cursor.is_some() {
            let val = cursor.as_ref().unwrap().val;
            *counter.entry(val).or_insert(0) += 1;
            cursor = &mut cursor.as_mut().unwrap().next;
        }
        let mut res = Some(Box::new(ListNode::new(1)));
        let mut cursor = &mut res;
        while head.is_some() {
            let next = head.as_mut().unwrap().next.take();
            let val = head.as_ref().unwrap().val;
            match counter.get(&val) {
                Some(&c) if c == 1 => {
                    cursor.as_mut().unwrap().next = head;
                    cursor = &mut cursor.as_mut().unwrap().next;
                } ,
                _ => {}
            }
            head = next;
        }
        return res.unwrap().next;
    }
}
```

### 截取

```rust

// time: O(N)
// space: O(1)
impl Solution {
    pub fn delete_duplicates(head: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        if head.is_none() || head.as_ref().unwrap().next.is_none() {
            return head;
        }
        let mut res = Some(Box::new(ListNode::new(1)));
        let mut cursor = &mut res;
        let mut head = head;
        while head.is_some() {
            let mut next = head.as_mut().unwrap().next.take();
            let mut duplicate = false;
            while next.is_some() && next.as_ref().unwrap().val == head.as_ref().unwrap().val {
                next = next.as_mut().unwrap().next.take();
                duplicate = true;
            }
            if !duplicate {
                cursor.as_mut().unwrap().next = head;
                cursor = &mut cursor.as_mut().unwrap().next;
            }
            head = next;
        }
        return res.unwrap().next;
    }
}
```