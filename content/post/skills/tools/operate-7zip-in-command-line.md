---
title: "命令行操作 7zip (附脚本)"
slug: "operate-7zip-in-command-line"
tags: ["脚本"]
description: 用命令行操作7zip 实现批量压缩与解压缩
date: 2021-09-21T22:35:45
---

## 前言

### 概述

用命令行操作7zip 实现批量压缩与解压缩

本文适用于windows, 理论上略作修改也可适配其他系统，但没必要(有更好选择)。

### 参考

[参考教程\(打不开挂代理\)](https://www.dotnetperls.com/7-zip-examples)

### 原理

利用循环命令操作 `7z.exe` 文件，实现批量复制与解压缩


##  命令介绍

### 命令详情

   在 7zip 安装目录下(含有 7z.exe 文件)输入 `7z` 可查看命令详情

   ```shell
	7-Zip 20.02 alpha (x64) : Copyright (c) 1999-2020 Igor Pavlov : 2020-08-08

	Usage: 7z <command> [<switches>...] <archive_name> [<file_names>...] [@listfile]

	<Commands>
	  a : Add files to archive
	  b : Benchmark
	  d : Delete files from archive
	  e : Extract files from archive (without using directory names)
	  h : Calculate hash values for files
	  i : Show information about supported formats
	  l : List contents of archive
	  rn : Rename files in archive
	  t : Test integrity of archive
	  u : Update files to archive
	  x : eXtract files with full paths

	<Switches>
	  -- : Stop switches and @listfile parsing
	  -ai[r[-|0]]{@listfile|!wildcard} : Include archives
	  -ax[r[-|0]]{@listfile|!wildcard} : eXclude archives
	  -ao{a|s|t|u} : set Overwrite mode
	  -an : disable archive_name field
	  -bb[0-3] : set output log level
	  -bd : disable progress indicator
	  -bs{o|e|p}{0|1|2} : set output stream for output/error/progress line
	  -bt : show execution time statistics
	  -i[r[-|0]]{@listfile|!wildcard} : Include filenames
	  -m{Parameters} : set compression Method
		-mmt[N] : set number of CPU threads
		-mx[N] : set compression level: -mx1 (fastest) ... -mx9 (ultra)
	  -o{Directory} : set Output directory
	  -p{Password} : set Password
	  -r[-|0] : Recurse subdirectories
	  -sa{a|e|s} : set Archive name mode
	  -scc{UTF-8|WIN|DOS} : set charset for for console input/output
	  -scs{UTF-8|UTF-16LE|UTF-16BE|WIN|DOS|{id}} : set charset for list files
	  -scrc[CRC32|CRC64|SHA1|SHA256|*] : set hash function for x, e, h commands
	  -sdel : delete files after compression
	  -seml[.] : send archive by email
	  -sfx[{name}] : Create SFX archive
	  -si[{name}] : read data from stdin
	  -slp : set Large Pages mode
	  -slt : show technical information for l (List) command
	  -snh : store hard links as links
	  -snl : store symbolic links as links
	  -sni : store NT security information
	  -sns[-] : store NTFS alternate streams
	  -so : write data to stdout
	  -spd : disable wildcard matching for file names
	  -spe : eliminate duplication of root folder for extract command
	  -spf : use fully qualified file paths
	  -ssc[-] : set sensitive case mode
	  -sse : stop archive creating, if it can't open some input file
	  -ssp : do not change Last Access Time of source files while archiving
	  -ssw : compress shared files
	  -stl : set archive timestamp from the most recently modified file
	  -stm{HexMask} : set CPU thread affinity mask (hexadecimal number)
	  -stx{Type} : exclude archive type
	  -t{Type} : Set type of archive
	  -u[-][p#][q#][r#][x#][y#][z#][!newArchiveName] : Update options
	  -v{Size}[b|k|m|g] : Create volumes
	  -w[{path}] : assign Work directory. Empty path means a temporary directory
	  -x[r[-|0]]{@listfile|!wildcard} : eXclude filenames
	  -y : assume Yes on all queries
   ```

### 关键命令

1. `a` 即 `add 或 archive`  压缩命令

   `7z a example.7z test.txt` 可将当前目录下 test.txt 文件压缩到 example.7z 文件

2. `e` 即 extra 解压命令(会破坏压缩包文件结构)

3. `x` 即 eXtra 解压命令(保留压缩包文件结构)

4. `-p` 添加密码参数

   `7z a example.7z test.txt -p123456` 会将 test.txt 文件压缩到 example.7z 且密码为 `123456`

   `7z e example.7z -p123456` 使用密码`123456` 解压 

## 脚本内容

### 一键压缩

   ```shell
    :: archive.bat

	@echo off
	for %%f in (*) do CALL :run %%f
	for /d %%d in (*) do CALL :run %%d

	:run
	echo %~1
	echo %~1|findstr ".bat" > nul && echo bat file || CALL :7zArchive %~1
	EXIT /B 0

	:7zArchive 
	if not %~dnp1=="" (
		path a "%~dnp1.7z" %~1 -ppassword
	)
	EXIT /B 0
   ```

### 一键解压缩

   ```shell
   	:: extra.bat

	for %%z in (*.7z, *.zip, *.rar) do path x %%z -ppassword
   ```

### 使用方法

1. 新建`extra.bat` 或 `archive.bat`, 将对应内容复制到对应文件内
2. 将脚本内path更改为`7z.exe`文件路径
   
   示例  `C:\Program Files\7-Zip\7z.exe`

3. 默认设定密码为 `password` 可自行更改, 注意解压时密码匹配
