---
title: "LRU缓存"
date: 2023-08-13T22:35:24+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [ LRU 缓存](https://leetcode.cn/problems/lru-cache/)

请你设计并实现一个满足  LRU (最近最少使用) 缓存 约束的数据结构。
实现 LRUCache 类：
- LRUCache(int capacity) 以 正整数 作为容量 capacity 初始化 LRU 缓存
- int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
- void put(int key, int value) 如果关键字 key 已经存在，则变更其数据值 value ；如果不存在，则向缓存中插入该组 key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字。
函数 get 和 put 必须以 O(1) 的平均时间复杂度运行。

## 题目解析

需要两个东西
- `list`: 记录访问顺序
- `map`: 记录存储结果

```rust

use std::rc::Rc;
use std::cell::RefCell;
use std::collections::HashMap;

struct Node {
    key: i32,
    val: i32,
    prev: Option<Rc<RefCell<Node>>>,
    next: Option<Rc<RefCell<Node>>>
}

impl Node {
    fn new(key: i32, val: i32) -> Self {
        Self {
            key,
            val,
            prev: None,
            next: None
        }
    }
}

struct List {
    head: Option<Rc<RefCell<Node>>>,
    tail: Option<Rc<RefCell<Node>>>
}

impl List {
    fn new() -> Self {
        Self {
            head: None,
            tail: None
        }
    }

    fn push_back(&mut self, node: Option<Rc<RefCell<Node>>>) {
        if let Some(tail) = self.tail.take() {
            if let Some(node) = &node {
                tail.as_ref().borrow_mut().next = Some(node.clone());
                node.as_ref().borrow_mut().prev = Some(tail);
            }
        } else {
            self.head = node.clone();
        }
        self.tail = node;
    }

    fn pop_front(&mut self) -> Option<Rc<RefCell<Node>>> {
        if let Some(head) = self.head.take() {
            if let Some(remain) = head.as_ref().borrow_mut().next.take() {
                remain.as_ref().borrow_mut().prev = None;
                self.head = Some(remain);
            } else {
                self.head = None;
                self.tail = None;
            }
            return Some(head);
        } else {
            return None;
        }
    }

    fn remove(&mut self, node: Option<Rc<RefCell<Node>>>) -> Option<Rc<RefCell<Node>>> {
        if let Some(node) = &node {
            let mut prev = node.as_ref().borrow_mut().prev.take();
            let mut next = node.as_ref().borrow_mut().next.take();
            if let Some(p) = &prev {
                p.as_ref().borrow_mut().next =  next.clone();
            } else {
                self.head = next.clone();
            }
            if let Some(n) = &next {
                n.as_ref().borrow_mut().prev = prev;
            } else {
                self.tail = prev;
            }
            return Some(node.clone());
        } else {
            return None;
        }
    }
}

struct LRUCache {
    cap: i32,
    used: i32,
    map: HashMap<i32, Rc<RefCell<Node>>>,
    list: List
}

impl LRUCache {
    fn new(cap: i32) -> Self {
        Self {
            cap,
            used: 0,
            list: List::new(),
            map: HashMap::new()
        }
    }

    fn get(&mut self, key: i32) -> i32 {
        if let Some(node) = self.map.get(&key) {
            let val = node.as_ref().borrow().val;
            let node = self.list.remove(Some(node.clone()));
            self.list.push_back(node);
            return val;
        } else {
            return -1;
        }
    }

    fn put(&mut self, key: i32, val: i32) {
        if let Some(node) = self.map.get(&key) {
            node.as_ref().borrow_mut().val = val;
            let node = self.list.remove(Some(node.clone()));
            self.list.push_back(node);
        } else {
            if self.used == self.cap {
                if let Some(node) = self.list.pop_front() {
                    self.map.remove(&node.as_ref().borrow().key);
                    self.used -= 1;
                }
            }
            let node = Rc::new(RefCell::new(Node::new(key, val)));
            self.map.insert(key, node.clone());
            self.list.push_back(Some(node));
            self.used += 1;
        }
    }
}
```

