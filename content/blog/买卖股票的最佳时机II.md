---
title: "买卖股票的最佳时机II"
date: 2023-07-26T23:17:50+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)

给你一个整数数组 prices ，其中 prices[i] 表示某支股票第 i 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 最多 只能持有 一股 股票。你也可以先购买，然后在 同一天 出售。

返回 你能获得的 最大 利润 。

>- 输入：prices = [7,1,5,3,6,4]
>- 输出：7

## 题目解析

一直上涨持有就行，问题是遇见下跌怎么卖。

我们假设有`A`、`B`、`C`、`D`四个点，其中
- `A` > `B`
- `B` > `C`
- `D` > `B`

考虑两个收益
- `D` - `B` + `B` - `A` = `D` - `A`
- `D` - `C` + `B` - `A`

可以看到，如果中途下跌，直接卖出肯定是赚的，答案就很简单了，只要遇见下跌卖出就行了。

> 注意边界：最后一天记得卖出

```rust

// time: O(N)
// space: O(1)
impl Solution {
    pub fn max_profit(prices: Vec<i32>) -> i32 {
        let (mut res, mut min, len) = (0, prices[0], prices.len());
        for i in 1..len {
            // 持续上涨
            if prices[i] > prices[i - 1] {
                // 最后一天卖出
                if i == len - 1 {
                    res += prices[i] - min;
                }
            } else {
                // 卖出昨天的
                res += prices[i - 1] - min;
                // 买入今天的；反正卖出也不影响
                min = prices[i];
            }
        }
        return res;
    }
}
```


