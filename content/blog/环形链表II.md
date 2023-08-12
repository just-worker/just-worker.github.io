---
title: "环形链表II"
date: 2023-08-12T18:52:56+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。 如果链表无环，则返回 null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

不允许修改 链表。

>- 输入：head = [3,2,0,-4], pos = 1
>- 输出：返回索引为 1 的链表节点
>- 解释：链表中有一个环，其尾部连接到第二个节点。

## 题目解析

当然，用`set`是最简单也是最直观的办法，但是我们还可以使用数学。

这里使用快慢指针，如果`fast`指针到了末尾，那就啥事都没有，现在考虑存在环的场景。

假设环的长度为`C`，不入环的长度为`k`，相遇的节点距离入环点距离为`b`, 我们可以得到如下关系
- `S(fast) = k + mC + b`
- `S(slow) = k + nC + b`
- `S(fast) = 2S(slow)`

简单的计算之后可以得到`k`和`C`的关系
- `S(fast) = 2S(slow)`
- `k + mC + b= 2nC + 2k + 2b`
- `mC = 2nC + k + b`
- `k = (m-2n)C - b`

其中`b`就是快慢指针相遇的节点，可以忽略；该等式表明
> 两个节点，一个从起始位置，一个从快慢指针相较点开始同步前进，最后一定相遇在`k`点，也即是入环点。


```java

// time: O(N)
// space: O(1)
public class Solution {
    public ListNode detectCycle(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode slow = head, fast = head;
        while (fast != null) {
            fast = fast.next;
            if (fast == null) {
                return null;
            }
            fast = fast.next;
            slow = slow.next;
            if (fast == slow) {
                fast = head;
                while (fast != slow) {
                    fast = fast.next;
                    slow = slow.next;
                }
                return fast;
            }
        }
        return null;
    }
}
```


