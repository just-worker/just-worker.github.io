---
title: "买卖股票的最佳时机III"
date: 2023-07-26T23:30:51+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/)

给定一个数组，它的第 i 个元素是一支给定的股票在第 i 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 两笔 交易。

注意：你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

>- 输入：prices = [3,3,5,0,0,3,1,4]
>- 输出：6
>- 解释：在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。

## 题目解析

这道题和之前的股票买卖不太一样
- 买卖次数限定为2
- 两次收益挂钩

因此不能直接计算单次收益，必须同时计算两次收益。

我们关注一下我们分别
- 第一次买入
- 第一次卖出
- 第二次买入
- 第二次卖出

我们的账户余额

```rust

// time: O(N)
// space: O(1)
impl Solution {
    pub fn max_profit(prices: Vec<i32>) -> i32 {
        let (mut buy1_balance, mut sell1_balance, mut buy2_balance, mut sell2_balance) = (-prices[0], 0, -prices[0], 0);
        for price in prices {
            // 买入一手后保持高账户余额
            buy1_balance = std::cmp::max(buy1_balance, -price);
            // 卖出一手后保持高账户余额
            sell1_balance = std::cmp::max(sell1_balance, buy1_balance + price);
            // 买入二手后保持高账户余额
            buy2_balance = std::cmp::max(buy2_balance, sell1_balance - price);
            // 卖出二手后保持高账户余额
            sell2_balance = std::cmp::max(sell2_balance, buy2_balance + price);
        }
        // 卖出二手后的账户余额
        return sell2_balance;
    }
}
```

