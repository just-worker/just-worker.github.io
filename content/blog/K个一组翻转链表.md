---
title: "K个一组翻转链表"
date: 2023-06-14T22:51:21+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [K个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

>- 输入：head = [1,2,3,4,5], k = 2
>- 输出：[2,1,4,3,5]


## 题目解析

没办法，硬算
```rust
// time: O(N)
// space: O(1)
impl Solution {
    pub fn reverse_k_group(mut head: Option<Box<ListNode>>, k: i32) -> Option<Box<ListNode>> {
        let mut dummy = Some(Box::new(ListNode::new(1)));
        let mut tail = &mut dummy;
        'outer: while head.is_some() {
            let mut cursor = &mut head;
            for _ in 0..k {
                if cursor.is_none() {
                    tail.as_mut().unwrap().next = head.take();
                    break 'outer;
                }
                cursor = &mut cursor.as_mut().unwrap().next;
            }
            let mut reverse = Some(Box::new(ListNode::new(1)));
            let mut cursor = &mut reverse;
            for _ in 0..k {
                let mut remain = head.as_mut().unwrap().next.take();
                let mut added = cursor.as_mut().unwrap().next.take();
                head.as_mut().unwrap().next = added;
                cursor.as_mut().unwrap().next = head;
                head = remain; 
            }
            println!("{:?}-{:?}", tail, reverse);
            tail.as_mut().unwrap().next = reverse.unwrap().next;
            while tail.as_mut().unwrap().next.is_some() {
                tail = &mut tail.as_mut().unwrap().next;
            }
        }
        return dummy.unwrap().next;
    }
}
```

