---
title: "删除链表的倒数第N个结点"
date: 2023-06-11T23:08:08+08:00
draft: false
tags: ["leetcode", "算法"]
featured: true
---

## [删除链表的倒数第N个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)


>- 输入：head = [1,2,3,4,5], n = 2
>- 输出：[1,2,3,5]


## 题目解析

有点遗憾的是，由于`rust`语法的问题，是不能够同时存在一个实体的两个可变引用的，只能想办法绕过去。

除了`unsafe`之外，只能使用如下特殊办法。

### 镜像克隆

```rust
// time: O(N)
// space: O(N); 其实是O(1)，语法问题，没办法
impl Solution {
    pub fn remove_nth_from_end(head: Option<Box<ListNode>>, n: i32) -> Option<Box<ListNode>> {
        let mut dummy = Some(Box::new(ListNode {val: 1, next: head}));
        let mut fast = &mut dummy.clone();
        let mut slow = &mut dummy;
        
        for _ in 0..(n+1) {
            fast = &mut fast.as_mut().unwrap().next;
        }
        while fast.is_some() {
            slow = &mut slow.as_mut().unwrap().next;
            fast = &mut fast.as_mut().unwrap().next;
        }
        let mut target = slow.as_mut().unwrap().next.take();
        if target.is_some() && target.as_ref().unwrap().next.is_some() {
            slow.as_mut().unwrap().next = target.as_mut().unwrap().next.take();
        }
        return dummy.unwrap().next;
    }
}

```
### 长度探查

```rust
// time: O(N)
// space: O(1)
impl Solution {
    pub fn remove_nth_from_end(mut head: Option<Box<ListNode>>, n: i32) -> Option<Box<ListNode>> {
        let mut cursor = &mut head;
        let mut depth = 0;
        while cursor.is_some() {
            cursor = &mut cursor.as_mut().unwrap().next;
            depth += 1;
        }
        let mut dummy = Some(Box::new(ListNode{val: 1, next: head}));
        let mut cursor = &mut dummy;
        for _ in 0..(depth - n) {
            cursor = &mut cursor.as_mut().unwrap().next;
        }
        let mut target = cursor.as_mut().unwrap().next.take();
        if target.is_some() && target.as_mut().unwrap().next.is_some() {
            cursor.as_mut().unwrap().next = target.as_mut().unwrap().next.take();
        }
        return dummy.unwrap().next;
    }
}
```


### 递归

```rust
// time: O(N)
// space: O(N)
impl Solution {
    pub fn remove_nth_from_end(mut head: Option<Box<ListNode>>, n: i32) -> Option<Box<ListNode>> {
        if Self::recursive_remove_reverse_nth(&mut head, n) == n {
            return head.unwrap().next;
        }
        head
    }

    fn recursive_remove_reverse_nth(head: &mut Option<Box<ListNode>>, n: i32) -> i32 {
        if head.is_none() {
            return 0;
        }
        let mut next = &mut head.as_mut().unwrap().next;
        if next.is_none() {
            return 1;
        }
        let prev = Self::recursive_remove_reverse_nth(next, n);
        if prev == n {
            head.as_mut().unwrap().next = head.as_mut().unwrap().next.take().unwrap().next.take();
        }
        return prev + 1;
    }
}
```

## `rust`必知必会

拆分作用域，隔离两个可变引用(变量)，防止交叉修改导致语法不过。
