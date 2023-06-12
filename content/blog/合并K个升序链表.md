---
title: "合并K个升序链表"
date: 2023-06-12T23:41:11+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

```text
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

## 题目解析

这里存在两种分形
- 一个链表合并一个数组链表
- 数组链表合并

不过其中的基石都是两个数组合并，可以复用之前的[合并两个有序链表]()，这里主要实现第二种方式

```rust
// time: O(kn x logk)
// space: O(logk) // 递归深度
impl Solution {
    pub fn merge_k_lists(lists: Vec<Option<Box<ListNode>>>) -> Option<Box<ListNode>> {
        let len = lists.len();
        if len == 0 {
            return None;
        }
        if len == 1 {
            return lists[0].clone();
        }
        if len == 2 {
            return Self::merge_two_lists(lists[0].clone(), lists[1].clone());
        }
        let mid = len / 2;
        let (mut left, mut right) = (vec![], vec![]);
        for (i, l) in lists.into_iter().enumerate() {
            if i < mid {
                left.push(l);
            } else {
                right.push(l);
            }
        }
        return Self::merge_two_lists(
            Self::merge_k_lists(left), 
            Self::merge_k_lists(right));
    }

    pub fn merge_two_lists(mut list1: Option<Box<ListNode>>, mut list2: Option<Box<ListNode>>) -> Option<Box<ListNode>> {
        let mut dummy = Some(Box::new(ListNode::new(1)));
        let mut tail = &mut dummy;
        loop {
            match (list1, list2) {
                (Some(mut a), Some(mut b)) => {
                    if a.val < b.val {
                        list1 = a.next.take();
                        tail.as_mut().unwrap().next = Some(a);
                        tail = &mut tail.as_mut().unwrap().next;
                        list2 = Some(b);
                    } else {
                        list2 = b.next.take();
                        tail.as_mut().unwrap().next = Some(b);
                        tail = &mut tail.as_mut().unwrap().next;
                        list1 = Some(a);
                    }
                },
                (a, b) => {
                    tail.as_mut().unwrap().next = a.or(b);
                    break;
                }
            }
        }
        return dummy.unwrap().next;
    }
}

```


