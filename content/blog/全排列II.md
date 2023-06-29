---
title: "全排列II"
date: 2023-06-29T22:54:00+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [全排列II](https://leetcode.cn/problems/permutations-ii/)

给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。

>- 输入：nums = [1,1,2]
>- 输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]

## 题目分析

和[上一题](https://just-worker.github.io/blog/%E5%85%A8%E6%8E%92%E5%88%97/)类似，但是要求不重复。

目前已经遇到过很多类似的要求，固然可以通过`set`进行去重，但是凭空增加了无谓的消耗，这点需要时刻注意。

之所以会产生重复的序列，主要在于其中的重复数字。我们应该如何去控制这些重复的数呢。

前面的[这道题](https://just-worker.github.io/blog/%E7%BB%84%E5%90%88%E6%80%BB%E5%92%8Cii/)给了我们一些灵感，可以统计频次，控制使用数量。

不过这还是不对，重复的序列固然是因为数值的重复，但还有重复数值之间的顺序，如果只是保证了数量，并不能够完全的做到不重复。

使用数值的时候，我们应该立下如下两个规矩，才能达成目的

- 不能重复使用数据
- 必须依次使用数据，不能跳过

```rust

// time: O(N x N!)
// space: O(N)
impl Solution {
    pub fn permute_unique(mut nums: Vec<i32>) -> Vec<Vec<i32>> {
        let len = nums.len();
        nums.sort();
        let mut res = vec![];
        Self::generate(&mut vec![], &nums, &mut res, &mut vec![false; len], 0);
        return res;
    }

    fn generate(collect: &mut Vec<i32>, nums: &Vec<i32>, res: &mut Vec<Vec<i32>>, used: &mut Vec<bool>, n: usize) {
        if n == nums.len() {
            res.push(collect.iter().map(|&v|v).collect());
            return;
        }
        for i in 0..nums.len() {
            // 使用过的不能再使用
            // 只能依次使用
            if used[i] || (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) {
                continue;
            }
            used[i] = true;
            collect.push(nums[i]);
            Self::generate(collect, nums, res, used, n + 1);
            used[i] = false;
            collect.pop();
        }
    }
}
```

