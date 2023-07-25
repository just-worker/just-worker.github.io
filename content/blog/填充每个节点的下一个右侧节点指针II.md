---
title: "填充每个节点的下一个右侧节点指针II"
date: 2023-07-25T23:04:57+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/)

给定一个二叉树

```c
struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL 。

初始状态下，所有 next 指针都被设置为 NULL 。

![](https://assets.leetcode.com/uploads/2019/02/15/117_sample.png)

>- 输入：root = [1,2,3,4,5,null,7]
>- 输出：[1,#,2,3,#,4,5,7,#]

## 题目解析

和前一题相比，现在要找到最右侧的节点。

这里主要有三个改动

1. 左节点不一定连接右节点了，无右节点情况和右节点查找方式相同
2. 右节点必须一直查找`next`，直到到达边界
3. 需要优先遍历右侧

> 尤其是优先遍历右节点，因为如果右节点的链接关系没有维护的情况下，左节点的子节点遍历可能链接不到右侧

```java
// time: O(N)
// space: O(N)
class Solution {
    public Node connect(Node root) {
        if (root != null) {
             
            dfs(root, root.right, false);
            dfs(root, root.left, true);
           
        }
        return root;
    }

    void dfs(Node parent, Node current, boolean left) {
        if (current == null) {
            return;
        }
        if (left && parent.right != null) {
            current.next = parent.right;
        } else {
            Node next = parent.next;
            while (next != null) {
                if (next.left != null) {
                    current.next = next.left;
                    break;
                }
                if (next.right != null) {
                    current.next = next.right;
                    break;
                }
                next = next.next;
            }
        }
        dfs(current, current.right, false);
        dfs(current, current.left, true);
        
    }
}
```

