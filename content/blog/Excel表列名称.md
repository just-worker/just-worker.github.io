---
title: "Excel表列名称"
date: 2023-08-16T23:04:25+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [Excel表列名称](https://leetcode.cn/problems/excel-sheet-column-title/)

给你一个整数 columnNumber ，返回它在 Excel 表中相对应的列名称。

```txt
A -> 1
B -> 2
C -> 3
...
Z -> 26
AA -> 27
AB -> 28 
...
```

>- 输入：columnNumber = 1
>- 输出："A"

## 题目解析

具体的官话请移步[官方题解](https://leetcode.cn/problems/excel-sheet-column-title/solutions/849363/excelbiao-lie-ming-cheng-by-leetcode-sol-hgj4/)，我这里只是说说我的理解。


我的第一思路就是按照26进制进行计算，但是突然发现最大的问题是，没有`0`。

因为`A -> 1`，对应十进制的话就是`9 -> 11`，因为没有`0`。

但是，英文字符是连续的，它必然是满足一个进制的计算的，只不过，它没有`0`。但是它必然也是26进制。

因此，我们只要同样的消除掉`0`就好了，谈不上削足适履，相当于是换了一套变换关系

`1 -> 0 -> A`, 这样一来，就是纯正的26进制数了。

```rust
const TABLE: [char; 26] = [
    'A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N', 'O', 'P', 'Q', 'R', 'S',
    'T', 'U', 'V', 'W', 'X', 'Y', 'Z',
];
impl Solution {
    pub fn convert_to_title(column_number: i32) -> String {
        let mut res = String::new();
        let mut num = column_number as usize;
        while num > 0 {
            num -= 1;
            res.insert(0, TABLE[num % 26]);
            num /= 26;
        }
        return res;
    }
}
```

