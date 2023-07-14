---
title: "搜索旋转排序数组II"
date: 2023-07-14T23:47:00+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [搜索旋转排序数组II](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/)

已知存在一个按非降序排列的整数数组 nums ，数组中的值不必互不相同。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转 ，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,4,4,5,6,6,7] 在下标 5 处经旋转后可能变为 [4,5,6,6,7,0,1,2,4,4] 。

给你 旋转后 的数组 nums 和一个整数 target ，请你编写一个函数来判断给定的目标值是否存在于数组中。如果 nums 中存在这个目标值 target ，则返回 true ，否则返回 false 。

你必须尽可能减少整个操作步骤。

>- 输入：nums = [2,5,6,0,0,1,2], target = 0
>- 输出：true


## 题目解析

必定二分搜索，不过因为存在重复元素，无法判断区间的情况下，需要逐渐缩减区间，不能够直接折半

```rust

// time: O(N)
// space: O(1)
impl Solution {
    pub fn search(nums: Vec<i32>, target: i32) -> bool {
        let len = nums.len();
        if len == 0 {
            return false;
        }
        if len == 1 {
            return nums[0] == target;
        }
        if len == 2 {
            return nums[0] == target || nums[1] == target;
        }
        let (mut left, mut right) = (0,len - 1);
        while left < len && left < right {
            if nums[left] == target || nums[right] == target {
                return true;
            }
            let mid = (left + right) / 2;
            if nums[mid] == target {
                return true;
            }
            if nums[left] == nums[mid] || nums[mid] == nums[right] {
                left += 1;
                if right > 1 {
                    right -= 1;
                }
                continue;
            }
            // 左侧有序
            if nums[left] < nums[mid] {
                if nums[left] < target && target < nums[mid] {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            } else {// 右侧有序
                if nums[mid] < target && target < nums[right] {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }

        }
        return left < len && nums[left] == target;
    }
}
```


