---
title: "[模板] 数据结构"
slug: "template-data-structure"
tags: ["模板"]
description:
date: 2021-11-24T13:55:51
---

## 数据结构

### 数组模拟数据结构

#### 队列

```c++

// pos 数组用到哪 ne -> next, val -> value

int head, pos, ne[MAXN], val[MAXN];

void init() {
    head = -1; pos = 0;
}

// 将值插入到头节点
void add_to_head(int x) {
   val[pos] = x; ne[pos] = head; head = pos++; 
}

// 删除头节点
void remove_head() {
    if(head != -1) head = ne[head];
}
```

