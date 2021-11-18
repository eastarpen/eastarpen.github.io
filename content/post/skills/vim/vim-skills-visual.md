---
title: "vim 实用技巧(二) visual 模式"
slug: "vim-skills-visual"
tags: ['vim']
description:
drate: 2021-09-20T19:05:21
draft: true
---

## visual vim 可视模式

    选中指定文本，并对其进行修改、复制、删除，共三种模式

## 常用命令

1. `v` 进入面向字符的可视模式
2. `V` 进入面向行的可视模式
3. `<c-v>` 进入面向块的可视模式
4. `gv` 重复上次选中区域
5. `.` 重复上次对选中区域操作
   
   行模式下有用，其他模式下操作相同数量字符起作用

6. `o` 切换选中区域两端

注意: visual 模式移动指令通用，`i` 改为 `I`, `a` 改为 `A`

## 块模式

### 编辑表格数据

将如下数据

```shell
Chapter     Page 
Normal      mode 15 
Insert      mode 31 
Visual      mode 44
```

修改为如下数据

```shell
Chapter     |   Page 
-------------------- 
Normal mode |   15 
Insert mode |   31 
Visual mode |   44 
```
### 修改列文本

将下面所有数据中的`/images` 修改为`/components`

```
li.one a{ background-image: url('/images/sprite.png'); } 
li.two a{ background-image: url('/images/sprite.png'); } 
li.three a{ background-image: url('/images/sprite.png'); }
```

### 在长短不一的文件后添加内容

在下面这段文本每一行后添加分号

```缓冲区内容
var foo = 1
var bar = 'a' 
Normal mode var foobar = foo + bar shell
```
