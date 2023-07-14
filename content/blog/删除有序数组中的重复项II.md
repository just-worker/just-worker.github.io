---
title: "删除有序数组中的重复项II"
date: 2023-07-14T23:34:26+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [删除有序数组中的重复项II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)

给你一个有序数组 nums ，请你 原地 删除重复出现的元素，使得出现次数超过两次的元素只出现两次 ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。

>- 输入：nums = [1,1,1,2,2,3]
>- 输出：5, nums = [1,1,2,2,3]
>- 解释：函数应返回新长度 length = 5, 并且原数组的前五个元素被修改为 1, 1, 2, 2, 3 。 不需要考虑数组中超出新长度后面的元素。

## 题目解析

1. 数值计数
2. 大于2跳过

```rust

// time: O(N)
// space: O(1)
impl Solution {
    pub fn remove_duplicates(nums: &mut Vec<i32>) -> i32 {
        let len = nums.len();
        if len < 3 {
            return len as i32;
        }
        let (mut left, mut count, mut last) = (0, 0, nums[0]);
        for i in 0..len {
            if nums[i] == last {
                count += 1;
            } else {
                count = 1;
                last = nums[i];
            }
            if count < 3 {
                nums.swap(left, i);
                left += 1;
            }
        }
        return left as i32;
    }
}
```

