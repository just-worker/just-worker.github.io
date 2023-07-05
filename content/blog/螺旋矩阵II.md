---
title: "螺旋矩阵II"
date: 2023-07-06T00:16:50+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [螺旋矩阵II](https://leetcode.cn/problems/spiral-matrix-ii/)

给你一个正整数 n ，生成一个包含 1 到 n2 所有元素，且元素按顺时针顺序螺旋排列的 n x n 正方形矩阵 matrix 。

![](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

>- 输入：n = 3
>- 输出：[[1,2,3],[8,9,4],[7,6,5]]


## 题目解析

- 方向转变：`right` >>> `down` >>> `left` >>> `up` >>> `right`
- 数值计算：
    1. 当前方向下一位可行，沿着该方向进行前进
    2. 当前方向下一位不行，转向计算下一位
    3. 转向后可行，继续步骤1
    4. 转向后不行，遍历完成


```rust

// time : O(N ^ 2)
// space : O(1)
impl Solution {
    pub fn generate_matrix(n: i32) -> Vec<Vec<i32>> {
        if n == 0 {
            return vec![];
        }
        if n == 1 {
            return vec![vec![1]];
        }
        let n = n as usize;
        let mut res = vec![vec![0; n]; n];
        let mut v = 1;
        let mut pos = Some((0, 0));
        let mut direction = Direction::RIGHT;
        while let Some((x, y)) = pos {
            res[x][y] = v;
            v += 1;
            // 当前方向
            pos = Self::next(n, x, y, &res, &direction);
            // 可行，移动下一步
            if pos.is_some() {
                continue;
            }
            // 转变方向
            direction = direction.next();
            // 计算下一步
            pos = Self::next(n, x, y, &res, &direction);
            // 转向也不行，完成
            if pos.is_none() {
                break;
            }
        }
        return res;
    }

    fn next(
        n: usize,
        x: usize,
        y: usize,
        res: &Vec<Vec<i32>>,
        direction: &Direction,
    ) -> Option<(usize, usize)> {
        return match direction {
            Direction::RIGHT if y + 1 < n && res[x][y + 1] == 0 => Some((x, y + 1)),
            Direction::DOWN if x + 1 < n && res[x + 1][y] == 0 => Some((x + 1, y)),
            Direction::LEFT if y > 0 && res[x][y - 1] == 0 => Some((x, y - 1)),
            Direction::UP if x > 0 && res[x - 1][y] == 0 => Some((x - 1, y)),
            _ => None,
        };
    }
}

enum Direction {
    RIGHT,
    DOWN,
    LEFT,
    UP,
}

impl Direction {
    fn next(self) -> Direction {
        match self {
            Direction::RIGHT => Direction::DOWN,
            Direction::DOWN => Direction::LEFT,
            Direction::LEFT => Direction::UP,
            Direction::UP => Direction::RIGHT,
        }
    }
}

```

