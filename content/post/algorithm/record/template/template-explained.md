---
title: "模版 [含解释]"
slug: "template-explained"
tags: ["template"]
description:
date: 2021-11-25
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
##### 思考

* 为什么不将 ne[] 数组全置为 -1?

    `head` 初始化为 -1, 再插入时由于 `ne[pos] = head` 或 `ne[pos] = ne[k]` , -1 会向后传递至链表末。

* 如何判断链表为空?

    `return head==-1;` 删除时 -1 会向前传递, 当 `head` 值为 -1 时, 链表为空

#### 双端队列

```c++
int pos, l[MAXN], r[MAXN], val[MAXN];

void init() {
    pos = 2, l[1] = 0, r[0] = 1;
}

// 在下标为 k 的节点后(右边)插入值为 x 的节点 
void insert(int k, int x) {
    val[pos] = x; // 赋值
	l[pos] = k, r[pos] = r[k]; // 建立新节点的两条边
	l[r[k]] = pos; // 将节点 k 的右节点的左节点指向新节点 pos
	r[k] = pos++; // 将节点 k 的右节点指向新节点 pos 
}

// 在链表最左端插入节点
void add_to_left(int x) {
    insert(0, x); // 在虚拟的左端点后插入新节点
}

// 在链表最右端插入节点
void add_to_right(int x) {
    insert(l[1], x); // 在虚拟的右端点的左端点(实际的最右端点)右边插入新节点
}

// 删除下标为 k 的节点
void remove(int k) {
    r[l[k]] = r[k], l[r[k]] = l[k];
}
```
##### 思考

* 为什么初始化是`pos = 2, l[1] = 0, r[0] = 1;`?

    将 0, 1 作为虚拟的端点, 不存储值, 真正的链表从 2 开始

* 为什么初始化没有 -1?

    单链表中 -1 作用是标记链表结束, 在双端链表中, 起点和结束点是固定的(0, 1), 因此不需要再用 -1 标志链表结束点

	此外, 若将 `l[1]` 初始化为 -1, 会失去右端点(没有节点指向右端点), `r[0]` 同理。

* 如何判断链表为空?
    
	`l[1] == 0 ` 或 `r[0] == 1`

