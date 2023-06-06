---
title: "N字形变换"
date: 2023-06-06T22:49:42+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [N字形变换](https://leetcode.cn/problems/zigzag-conversion/)

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。

比如输入字符串为 "PAYPALISHIRING" 行数为 3 时，排列如下：

```text
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。
>- 输入：s = "PAYPALISHIRING", numRows = 3
>- 输出："PAHNAPLSIIGYIR"


## 题目分析

直接使用模拟的办法进行解析的话，明显会造成空间的浪费，这里就不说了，详细可以看[官网解答](https://leetcode.cn/problems/zigzag-conversion/solution/z-zi-xing-bian-huan-by-leetcode-solution-4n3u/)。


### 直接计算
我们可以思考的是，能够通过数学的规律，直接进行计算。

分行`lines`，明显观察到的规律就是，整个字符周期`batch = lines + lines - 2`，其中因为对称的第二列首尾被截取掉了。


其次，在向上的填充，有对称的部分，很轻易能够计算出，他们的位置其实就是关于`lines`的对称。


现在，分为`lines`行，我们直接计算一下。

```rust
// time: O(N)
// space: O(N)
impl Solution {
    pub fn convert(s: String, rows: i32) -> String {
        let lines = rows as usize;
        if lines < 2 || s.len() < lines {
            return s;
        }
        let batch = lines + lines - 2;
        let mut res = vec![vec![]; lines];
        for (i, ch) in s.chars().enumerate() {
            let idx = i % batch;
            if idx < lines {
                res[idx].push(ch);
            } else {
                res[batch - idx].push(ch);
            }
        }
        res.concat().iter().collect()
    }
}

```

### 直接生成

直接计算已经非常不错了，核心关系我们也找到了，能够继续优化呢。


如果我们直接生成结果，而不用借助多个数组，会怎么样子。可以预见的是，我们将遍历不止一次，重复遍历的次数为`lines x N`，时间复杂度并不会涨，但是空间复杂度会降低到`O(1)`，因为生成的就是结果本身，不算辅助空间。

```rust
// time: O(N)
// space: O(1)
impl Solution {
    pub fn convert(s: String, rows: i32) -> String {
        let lines = rows as usize;
        if lines < 2 || s.len() < lines {
            return s;
        }
        let batch = lines + lines - 2;
        let chars : Vec<char> = s.chars().collect();
        let mut res = String::new();
        for offset in 0..lines {
            let mut factor = 0;
            loop {
                let begin = factor * batch;
                if begin > chars.len()  {
                    break;
                }
                if begin + offset < chars.len() {
                    res.push(chars[begin + offset]);
                }
                let mirror = batch - offset;
                // 独行不重复计算
                if offset != 0 && mirror != offset && begin + mirror < chars.len(){
                    res.push(chars[begin + mirror]);
                }
                factor += 1;
            }
        }
        return res;
    }
}

```

这里主要的变异点在于
- 需要重复遍历`lines`遍
- 有些行的遍历，一个周期内需要添加两个字符
