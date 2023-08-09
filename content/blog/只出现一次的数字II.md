---
title: "只出现一次的数字II"
date: 2023-08-09T22:48:03+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [只出现一次的数字 II](https://leetcode.cn/problems/single-number-ii/description/)

给你一个整数数组 nums ，除某个元素仅出现 一次 外，其余每个元素都恰出现 三次 。请你找出并返回那个只出现了一次的元素。

你必须设计并实现线性时间复杂度的算法且不使用额外空间来解决此问题。

>- 输入：nums = [2,2,3,2]
>- 输出：3

## 题目解析

说实话，没有太好的头绪，这里是抄的[官方题解](https://leetcode.cn/problems/single-number-ii/solutions/746993/zhi-chu-xian-yi-ci-de-shu-zi-ii-by-leetc-23t6/)，值得赏读。

### 思维突破

基础的办法而言，脱离哈希表实在是没办法。但是官方的位运算思路很值得借鉴。

> 对于单个位而言，三个相同的数对3取余自然会抵消。

```rust
impl Solution {
    pub fn single_number(nums: Vec<i32>) -> i32 {
        let mut res = 0;
        for i in 0..32 {
            let mut bit_sum = 0;
            for &num in nums.iter() {
                bit_sum += (num >> i) & 1;
            }
            if bit_sum % 3 > 0{
                res = res | (1 << i);
            }
        }
        return res;
    }
}
```

### 换个体系

现在的操作已经变成了位运算的体系，因此，不至于要逐位进行计算，需要一个电路进行直接计算。

具体的演算请查看[官方题解](https://leetcode.cn/problems/single-number-ii/solutions/746993/zhi-chu-xian-yi-ci-de-shu-zi-ii-by-leetc-23t6/)，这里只记录一下方案。

```rust
impl Solution {
    pub fn single_number(nums: Vec<i32>) -> i32 {
        let (mut a, mut b) = (0, 0);
        for num in nums {
            b = !a & (b ^ num);
            a = !b & (a ^ num);
        }
        return b;
    }
}
```

