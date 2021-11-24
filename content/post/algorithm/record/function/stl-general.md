---
title: "STL初步"
slug: "stl-general"
tags: ["竞赛"]
description:
date: 2021-10-15T10:43:21
---

## algorithm

### sort

1. sort 是个模板函数，对于非内置数据类型需要定义运算符.
2. `sort(v.begin(), v.end())`

### lower_bound

1. `int pos = lower_bound(arr, arr+n, x) `
2. 查找大于或等于 `x` 的第一个位置 
3. 排序后查找
4. 下面代码输出 0 

   ```c++
	int arr[] = {2, 2, 3, 4, 5};
	cout << lower_bound(arr, arr+5, -1) - arr;
	return 0;
   ```
