---
title: "反转链表II"
date: 2023-07-18T23:32:30+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)

给你单链表的头指针 head 和两个整数 left 和 right ，其中 left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表

![](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

>- 输入：head = [1,2,3,4,5], left = 2, right = 4
>- 输出：[1,4,3,2,5]


## 题目解析

暴力计算即可

```rust
// time: O(N)
// space: O(1)
impl Solution {
    pub fn reverse_between(head: Option<Box<ListNode>>, left: i32, right: i32) -> Option<Box<ListNode>> {
        if head.is_none() || left == right || head.as_ref().unwrap().next.is_none() {
            return head;
        }
        let mut head = Some(Box::new(ListNode{
            val: 501,
            next: head
        }));
        let mut head_cursor = &mut head;
        // 从1开始计算的
        for _ in 1..left {
            head_cursor = &mut head_cursor.as_mut().unwrap().next;
        }
        // 逆转中部
        let mut remain = head_cursor.as_mut().unwrap().next.take();
        for _ in left..=right {
            let head_next = head_cursor.as_mut().unwrap().next.take();
            let remain_next = remain.as_mut().unwrap().next.take();
            remain.as_mut().unwrap().next = head_next;
            head_cursor.as_mut().unwrap().next = remain;
            remain = remain_next;
        }
        // 滑动到尾部
        while head_cursor.as_ref().unwrap().next.is_some() {
            head_cursor = &mut head_cursor.as_mut().unwrap().next;
        }
        // 接续 
        head_cursor.as_mut().unwrap().next = remain;
        return head.unwrap().next;
    }
}
```

