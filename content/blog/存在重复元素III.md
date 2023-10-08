---
title: "存在重复元素III"
date: 2023-10-08T22:28:02+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [存在重复元素 III](https://leetcode.cn/problems/contains-duplicate-iii/)

给你一个整数数组 nums 和两个整数 indexDiff 和 valueDiff 。

找出满足下述条件的下标对 (i, j)：

- i != j,
- abs(i - j) <= indexDiff
- abs(nums[i] - nums[j]) <= valueDiff

如果存在，返回 true ；否则，返回 false 。

## 题目解析

滑动窗口的提示很明显，其中区间的话可以用桶算法。

```rust


// time: O(N)
// space: O(N)
impl Solution {
    pub fn contains_nearby_almost_duplicate(nums: Vec<i32>, index_diff: i32, value_diff: i32) -> bool {
        if nums.is_empty() || index_diff == 0 {
            return false;
        }
        let generate = |v: i32|{
            if v >= 0 {
                return v / (value_diff + 1);
            }
            return (v + 1) / (value_diff + 1) - 1;
        };
        let mut index_diff = nums.len().min(index_diff as usize);
        let mut bucket = std::collections::HashMap::<i32, i32>::new();
        for (idx, &value) in nums.iter().enumerate() {
            let id = generate(value);
            if bucket.contains_key(&id) {
                return true;
            }
            if let Some(&another) = bucket.get(&(id - 1)) {
                if (another - value).abs() <= value_diff {
                    return true;
                }
            }
            if let Some(&another) = bucket.get(&(id + 1)) {
                if (another - value) .abs() <= value_diff {
                    return true;
                }
            }
            bucket.insert(id, value);
            if idx >= index_diff {
                bucket.remove(&generate(nums[idx - index_diff]));
            }
        }
        return false;
    }
}
```



