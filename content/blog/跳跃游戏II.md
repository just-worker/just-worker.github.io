---
title: "跳跃游戏II"
date: 2023-06-27T22:55:42+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [跳跃游戏II](https://leetcode.cn/problems/jump-game-ii/)

给定一个长度为 n 的 0 索引整数数组 nums。初始位置为 nums[0]。

每个元素 nums[i] 表示从索引 i 向前跳转的最大长度。换句话说，如果你在 nums[i] 处，你可以跳转到任意 nums[i + j] 处:

- 0 <= j <= nums[i] 
- i + j < n
- 返回到达 nums[n - 1] 的最小跳跃次数。生成的测试用例可以到达 nums[n - 1]。

>- 输入: nums = [2,3,1,1,4]
>- 输出: 2
>- 解释: 跳到最后一个位置的最小跳跃数是 2。
     从下标为 0 跳到下标为 1 的位置，跳 1 步，然后跳 3 步到达数组的最后一个位置。


## 题目解析

### 直接爆破
```rust


// time: O(N^2)
// space: O(N)
impl Solution {
    pub fn jump(nums: Vec<i32>) -> i32 {
        let len = nums.len();
        let mut dp = vec![10000000; len];
        dp[0] = 0;
        for i in 0..len {
            for j in 1..=nums[i] as usize {
                if i + j < len {
                    dp[i + j] = std::cmp::min(dp[i + j], 1 + dp[i]);
                }
            }
        }
        return dp[len - 1];

    }
}
```


### 反向查找

从最后开始算，每次都反向调到最大的步子

```rust


// time: O(N^2)
// space: O(1)
impl Solution {
    pub fn jump(nums: Vec<i32>) -> i32 {
        let len = nums.len();
        let mut end = len - 1;
        let mut steps = 0;
        while end > 0 {
            for i in 0..end {
                if i + nums[i] as usize >= end {
                    end = i;
                    steps += 1;
                    break;
                }
            }
        }
        return steps;
    }
}
```


### 边界推进

反向的话，需要遍历之前的边界，而且没有记忆，能不能从正向进行计算呢。

不过首先得厘清一个问题：正向是否能够`贪心`的按照最大步子进行跳跃。

答案是可以的，但原因却不是`贪心`：
- 最大步子跳过了最优解，但其实是可以跳到最优解的，且连续大步因为拆分需要`+1`，两者是等价的
- 根据前一条，每次按照最大步跳，到达边界逐步推进，因为自动兼容最优解，两者结果等价

```rust

// time: O(N)
// space: O(1)
impl Solution {
    pub fn jump(nums: Vec<i32>) -> i32 {
        let (len, mut end, mut max_right, mut steps) = (nums.len(), 0, 0, 0);
        // end >= i，大于的时候肯定会越过一步，等于的时候会再+1，因此不用考虑 len - 1 最后一步
        for i in 0..len - 1 {
            // 最右边界
            max_right = std::cmp::max(max_right, i + nums[i] as usize);
            // 到达边界
            if i == end {
                // 继续跳到最右边界
                end = max_right;
                // 每次跳动+1
                steps += 1;
            }
        }
        return steps;
    }
}
```


