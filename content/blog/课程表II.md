---
title: "课程表II"
date: 2023-09-09T21:25:10+08:00
draft: false
tags: ["leetcode", "算法"]
---

## [课程表 II](https://leetcode.cn/problems/course-schedule-ii/)

现在你总共有 numCourses 门课需要选，记为 0 到 numCourses - 1。给你一个数组 prerequisites ，其中 prerequisites[i] = [ai, bi] ，表示在选修课程 ai 前 必须 先选修 bi 。

例如，想要学习课程 0 ，你需要先完成课程 1 ，我们用一个匹配来表示：[0,1] 。
返回你为了学完所有课程所安排的学习顺序。可能会有多个正确的顺序，你只要返回 任意一种 就可以了。如果不可能完成所有课程，返回 一个空数组 。

>- 输入：numCourses = 2, prerequisites = [[1,0]]
>- 输出：[0,1]
>- 解释：总共有 2 门课程。要学习课程 1，你需要先完成课程 0。因此，正确的课程顺序为 [0,1] 。


## 题目解析

和之前的步骤一样

```rust

impl Solution {
    pub fn find_order(num_courses: i32, prerequisites: Vec<Vec<i32>>) -> Vec<i32> {
        if num_courses == 0 || prerequisites.is_empty() {
            return (0..num_courses).collect();
        }
        let num_courses = num_courses as usize;
        let (mut lock, mut unlock) = (vec![0; num_courses], vec![vec![]; num_courses]);
        for pair in prerequisites.iter() {
            lock[pair[0] as usize] += 1;
            unlock[pair[1] as usize].push(pair[0] as usize);
        }
        let mut free = (0..num_courses)
            .filter(|&v| lock[v] == 0)
            .collect::<Vec<usize>>();
        if free.len() == num_courses {
            return free.into_iter().map(|v| v as i32).collect();
        }
        let mut fidx = 0;
        while fidx < free.len() {
            for &course in &unlock[free[fidx]] {
                lock[course] -= 1;
                if lock[course] == 0 {
                    free.push(course);
                }
            }
            fidx += 1;
        }
        if fidx == num_courses {
            return free.into_iter().map(|v| v as i32).collect();
        }
        return vec![];
    }
}
```

