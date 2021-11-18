---
title: "vim 实用技巧 (一) vim 基础认知"
slug: "vim-skills-general"
tags: ['vim']
description:
date: 2021-09-20T19:05:21
draft: true
---

1. `vim -u NONE -N` 启动初始 `vim`
   
   `-u NONE` 告诉 vim 不加载 `vimrc` 内容
   `-N` 告诉 vim `Nocompatible` 不兼容 vi

2. `.` 命令 重复上次 ** 修改 ** `j, k, h, G` 之类的移动操作不记录

    实例：在每行末尾添加`;`

3. `>G` 添加当前行到文档末尾的缩进层次

   `>` 增加当前行的缩进层次 `<` 相反

   `> <` 正常工作需要 `:set shiftwidth=4 softtabstop=4 expandtab`

4. `;` 重复上一次查找(命令 `f`) `,` 回到上一次查找


   整体认知 `. 相对于 u`, `; 相对于 ,` ` n 相对于 N`

5. `*` 查找当前单词并自动跳入下一个
   
   常与 `cw . n` 配合使用

6. `daw` 删除一个单词
 
    `dw` 与 `daw` 区别 dw 删除光标之后

7. 数字加减
   
   `<C-a>` 正向查找下一个数字，将其值 `+1` `10<C-A>` `+ 10`

   `<c-x>` 减法

8. 大小写转换

   `g~` 转换为另一个
   
   `gu` 转换为小写

   `gU` 转换为大写8. 大小写转换

9. operator

   所有的 operator `d c y > < g~ gU gu` 

   重复所有的 operator 表示对当前行奏效

10. 插入模式下命令

    1. 删除
	  
	   `<c-w>` 删除至前一个单词

	   `<c-h>` 相当于 退格

	   `<c-u>` 删除至行首

	2. `<c-o>`

	    输入一个指令后返回插入模式
	
	3. `<c-r>0` 粘贴寄存器内容

       `<c-r>=` 使用寄存器表达式

11. `R` 替换模式，将输入内容和光标后内容逐渐字符替换

12. 可视模式(visual model)

    `o` 切换两端

	`gv` 正常模式 重选上次选中的内容.

13. 命令行模式

    `:t` t 即 cp 即 copy 

	`.` 当前行


	`6t.` 将第六行内容复制到当前行下方, 注意`t` 命令不会更改寄存器内容

	可以将 `t` 命令记忆为 `copy to`

14. 命令行模式下执行 普通模式命令

    `:% normal A;` 在每一行下面添加 `;`

	`'<,'> normal .` 在选中区域内执行上一次修改操作

## 文件

文件被打开到缓冲区, 通过 `:ls` 命令查看文件列表

   ```
   1      "setup.sh"                     line 4
   2 #    "test.txt"                     line 3
   3 %a + "vim-skills-01.md"   
   ```
% 表示当前显示的文件 # 表示轮换文件 `<c-^>` 轮换， `:bnext` 切换到下一个

切换文件命令`bprev, bnext, bfirst, blast, buffer 序号, buffer 路径正则`
