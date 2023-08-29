---
title: "买卖股票的最佳时机IV"
date: 2023-08-29T22:44:49+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/)

给你一个整数数组 prices 和一个整数 k ，其中 prices[i] 是某支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 k 笔交易。也就是说，你最多可以买 k 次，卖 k 次。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

>- 输入：k = 2, prices = [2,4,1]
>- 输出：2
>- 解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。

## 题目解析

和之前的计算财富最大值一样进行操作

```rust

impl Solution {
    pub fn max_profit(k: i32, prices: Vec<i32>) -> i32 {
        let days = prices.len();
        if days == 0 {
            return 0;
        }
        let init = i32::MIN / 2;
        // 有效操作最多就是一半
        let ops = (k as usize).min(days / 2) + 1;
        let (mut buy, mut sell) = (vec![vec![0; ops]; days], vec![vec![0; ops]; days]);
        // 初始化数据
        buy[0].iter_mut().for_each(|v| *v = init);
        sell[0].iter_mut().for_each(|v| *v = init);
        // 第一步交易买入
        buy[0][0] = -prices[0];
        sell[0][0] = 0;
        for day in 1..days {
            // 当天第一笔买入结果
            buy[day][0] = buy[day - 1][0].max(sell[day - 1][0] - prices[day]);
            for op in 1..ops {
                // 持有 或者 昨天卖出买入今天
                buy[day][op] = buy[day - 1][op].max(sell[day - 1][op] - prices[day]);
                // 持有 或者 昨天上一步卖出，今天买入
                sell[day][op] = sell[day - 1][op].max(buy[day - 1][op - 1] + prices[day]);
            }
        }
        return *sell.last().unwrap().iter().max().unwrap();
    }
}
```

