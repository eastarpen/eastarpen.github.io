---
title: "[acwing][2022]寒假每日一题week-2"
slug: "week-02"
tags: ["差分", "前缀和", "扫描线"]
description:
math: true
date: 2022-02-11
---

## 1969 品种接近

[题目链接](https://www.acwing.com/problem/content/1971/)


使用桶记录同id的上一头牛位置, 直接比较即可

## 1978 奶牛过马路

[题目链接](https://www.acwing.com/problem/content/description/1980/)

假设某条路径是从 a[i] -> b[i]

安全的条件是

$$
\forall i < x, a_i < a_x \iff a_x > max (a_1, ..., a_{x-1})
$$
$$
\forall j > x, a_j > a_x \iff a_x < min (a_{x+1}, ..., a_n)
$$

使用前缀后缀和即可

## 1987 粉刷栅栏

[题目链接](https://www.acwing.com/problem/content/1989/)

差分求解, 数据 1e9, 需要优化

因为只有 1e5 个区间, 最多 2e5 个区间

使用map离散化 `map<int, int> d`

d[l] 表示区间 [l, l+1] 被标记

则标记区间 [l, r] 操作为 `d[l]++, d[r]--;`

d[r+1]-- 表示标记区间 [r+1, r+2], 需要标记的是 [r, r+1]

也可看作一维扫描线


