---
title: "单词拆分II"
date: 2023-08-11T20:52:15+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [单词拆分 II](https://leetcode.cn/problems/word-break-ii/)


给定一个字符串 s 和一个字符串字典 wordDict ，在字符串 s 中增加空格来构建一个句子，使得句子中所有的单词都在词典中。以任意顺序 返回所有这些可能的句子。

注意：词典中的同一个单词可能在分段中被重复使用多次。

>- 输入:s = "catsanddog", wordDict = ["cat","cats","and","sand","dog"]
>- 输出:["cats and dog","cat sand dog"]

## 题目解析

和跳跃一样，有些不一定能跳跃到终点，但是从终点肯定能够判断是否可以到达原点。