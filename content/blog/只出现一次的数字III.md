---
title: "只出现一次的数字III"
date: 2023-10-13T22:48:42+08:00
draft: false
tags: ["leetcode", "算法"]
featured: true
---

## [只出现一次的数字 III](https://leetcode.cn/problems/single-number-iii/)

给你一个整数数组 nums，其中恰好有两个元素只出现一次，其余所有元素均出现两次。 找出只出现一次的那两个元素。你可以按 任意顺序 返回答案。

你必须设计并实现线性时间复杂度的算法且仅使用常量额外空间来解决此问题。

>- 输入：nums = [1,2,1,3,2,5]
>- 输出：[3,5]
>- 解释：[5, 3] 也是有效的答案。


## 题目解析

1. 相同的数据，两两异或相互抵消
2. 最后结果一定是目标两个数的异或结果
3. 其中二进制为1的地方必定是两个数据的分歧点
4. 以分歧点分组异或，最终一定能分离两个数

```rust

// time: O(N)
// space: O(1)
impl Solution {
    pub fn single_number(nums: Vec<i32>) -> Vec<i32> {
        let xor = nums.iter().fold(0, |xor, &v| xor ^ v);
        let bit = if xor == i32::MAX { xor } else {xor & -xor};
        let (mut v1, mut v2) = (0 , 0);
        for v in nums {
            if v & bit == 0 {
                v1 ^= v;
            } else {
                v2 ^= v;
            }
        }
        return vec![v1, v2];
    }
}
```

## 复习

- `x & (x - 1)`: 去掉最后一个1
- `x & -x`: 取最后一个1
