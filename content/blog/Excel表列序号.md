---
title: "Excel表列序号"
date: 2023-08-20T23:37:48+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [Excel 表列序号](https://leetcode.cn/problems/excel-sheet-column-number/)

给你一个字符串 columnTitle ，表示 Excel 表格中的列名称。返回 该列名称对应的列序号 。

```text
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```
>- 输入: columnTitle = "A"
>- 输出: 1


## 题目解析

26进制，但是缺乏`0`，
我们可以看做`A -> 0 -> 1`的变化进行映射，整体还是26进制，但是具体数值需要`offset`

```rust
impl Solution {
    pub fn title_to_number(column_title: String) -> i32 {
        let base = 'A' as u8;
        let mut res = 0;
        for &v in column_title.as_bytes() {
            res *= 26;
            res += (v - base + 1) as i32;
        }
        return res;
    }
}
```

