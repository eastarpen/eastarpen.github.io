---
title: "[blog] 使用 hugo 搭建个人博客"
slug: "build-blog-with-hugo"
tags: ['blog']
descriptipn: 零成本, 在 windows 下使用hugo 在github page 上搭建个人博客, 附脚本
date: 2021-09-18T18:28:57
---

## 前言

### 概述

1. 零成本，使用 hugo 搭建个人网站并部署在 GitHub Page 上。
2. 附脚本
3. 本文不能让你对 hugo 有全面的认知，由此需求请自行搜索教程或阅读官方文档
4. github 在国内使用体验极差, 可以转换到国内平台 Gitee, 教程通用
    
	对于有代理的用户通过 git 访问 GitHub 仍然没有速度，建议使用如下命令

	```shell
	git config --global http.proxy "socks5://127.0.0.1:7891"
    git config --global https.proxy "socks5://127.0.0.1:7891"
	```

	把后面的端口号改为你的端口号。

### 本文受众

1. 自己(备忘录)
2. 使用过 git 且对搭建个人博客有兴趣;
3. 未使用 git 但对 搭建个人博客有强烈需求的或爱折腾的;
4. 单纯的了解一下。

### 相关

1. [hugo 官网](https://gohugo.io/)
2. [git 下载](https://git-scm.com/downloads)

### 前置条件

搭建博客的必要条件，但与本文无关，不赘述。

1. 一个 [github](https://www.github.com) 账号;
2. 下载安装 git 并绑定 github 账号;

## 搭建博客

### 下载安装 hugo

网络上有很多下载安装 hugo 的方法, 这里给出最简单的。

1. 到 [github hugo release](https://github.com/gohugoio/hugo/releases/tag/v0.88.1) 下载 hugo 二进制文件压缩包。

   * windows 有四个版本，选择适合自己系统的下载即可。 其中带 `extended` 的是高级版本，建议下载它。(按照本文设置不用 extended 版会出错)

2. 解压下载的文件到某文件夹下并将此文件夹放入系统变量 `Path`中。

   * 放入 `Path` 方法

     ` win + s` -> 输入 `环境变量` 或 `environment variables` -> 点击 `环境变量` -> 在下方 `系统变量` 中找到 `Path` -> 双击 `Path` -> 在打开的列表中填入你的路径

   我解压到 `P:/blog/hugo` 文件夹下，则我填入的路径为`P:\blog\hugo`
   
   ```
   |--hugo
     |--LICENSE
     |--README.md
     |--hugo.exe
   ```
3. 打开终端(cmd 或 git bash 或 power shell) 输入 `hugo version` 显示版本号即安装正确。
   
### 使用 hugo 建立站点

1. 创建空白网站

    终端中输入 `hugo new site siteName` 即可在当前文件夹下创建名为 `siteName` 的文件夹，里面文件即为空白网站结构。

2. 下载主题

   可以在 [hugo](https://themes.gohugo.io/) 下载喜欢的主题, 主题介绍页面有下载方法。

   本文以 `hugo-theme-stack` 为例。

   按照[官方中文文档](https://docs.stack.jimmycai.com/zh/) 在网站根目录(即上文的 siteName 文件夹下) 使用命令 `git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack` 安装主题

3. 配置主题

   阅读主题官方说明文档学习个性化配置主题(下面有我的一些见解)

   嫌麻烦可以直接将主题录下`exapmleSite` 下的配置文件直接复制到自己网站根目录下(记得删除原来的)

4. 生成 html

   在网站根目录下 `siteName/` 下 输入 `hugo` 即可生成

### 网站结构的一些讲解

#### 根目录下文件

1. `./archetypes/default.md` 使用 `hugo new fileName` 创建文件时自动在新文件里写入的内容。
2. `content` 所有文章保存在这里。
3. `themes` 存放下载的主题
4. 其他, 基本用不到

#### config 文件

1. config 文件主要有 `config.toml` 或 `config.yaml` 两种，只是数据格式不一样，根据你的主题示例选择, 比如 `hugo-theme-stack` 要求`config.yaml`
2. config 文件中要有`theme: your theme`, 这样才能使用主题
   `config.toml` 则是 `theme = your theme`
3. `permalinks` 控制url的。
   
   假设 `content` 结构如下

   ```
   |--content
     |--page
	   |--search.md
	   |--about.md
	 |--post
	   |--test01.md
	   |--test02.md
    ```
	
    如果不对 `permalinks` 进行任何配置, 对应的四篇文章地址分别为

	```
	1. ./page/search
	2. ./page/about
	3. ./post/test01
	4. ./post/test02
    ```

	将 `permalinks` 配置为

	```
	permalinks:
	  page: /:slug/
	  post: /p/:slug/

	```
	则地址变为

	```
	1. ./search
	2. ./about
	3. ./p/test01
	4. ./p/test02
	```

	`:slug` 表示 对应文件的 `slug`  属性值, 不设置则为 `title`属性, `title` 属性默认为文件名

4. 没有 favicon 子段如何配置网页图标

   * 将`favicon.icon` 放在`siteName/static/img/` 目录下;
   * 将 `favicon.icon` 放在网站根目录下, 是通过 `hugo` 命令生成的网站， 即`public 或 docs` 目录下， 不是 `siteName/` 下

## 部署博客到 github page 上

1. 新建一个仓库, 仓库名为 `yourName.github.io`

    如果你的github 账号叫做 `tom` 那就新建 `tom.github.io`

2. 将 网站纳入git 管理

   即在 `siteName/` 下使用命令 `git init`

3. 将网站目录和仓库关联(前提: 本地git 和账号已绑定)
   
   `git remote add origin  url`

4. 通过`hugo -d docs` 生成网页到 `docs` 文件

5. `git push` 推送到远程仓库

6.  在你的`name.github.io` 仓库页面

    setting -> Pages -> 将分支 Branch 选为 master, 后面的文件选为 docs

7. 本部分要求对 git 有一定了解， 不会的建议自己搜索教程了解后再行尝试

## 个人使用脚本

### update

#### 介绍

在 `git bash` 终端中任意路径下输入 `update` 即可实现自动生成网页代码并推送到远程仓库。

#### 使用方法

1. 在`hugo` 目录下新建 `update.sh` 文件
2. 将`脚本内容` 复制到`update.sh` 中
3. 将脚本中 `path` 改为你的网站目录
   
   假如你的网站目录是 `p:/blog/tom` 则你填入的应该是 `/p/blog/tom`(Linux 样式)

4. 通过 `chmod +x update.sh` 命令为脚本添加可执行权限
5. 在 hugo 目录下输入 `./update.sh` 查看脚本是否运行
6. 通过`mv update.sh update` 将`update.sh` 文件改名为`update`

#### 注意

1. 脚本可在任意目录运行
2. 显示`update: command not find` 可能是脚本无可执行权限或脚本不在环境变量`Path`内
3. 脚本运行前提是已与远程仓库建立链接

#### 脚本内容 

```shell
cd path
rm -rf docs
hugo -d docs
git add .
git commit -m 'update blog'
git push
```

#### 逐行解释

```shell
到 path 目录下
删除 path 目录下 docs 文件夹及里面所有内容
生成网页到 docs 目录下
添加所有新内容到 git 缓存区
将所有内容添加到本地仓库
将本地仓库推送到远程仓库
 ```

### new

#### 介绍

在任意目录下输入`new` 可生成指定markdown 文件(即md文件)

#### 使用方法

和 `update` 一样，不过文件名变了

#### 脚本内容

```shell
write() {
    echo $1 >> $file
}

echo -n "please intput title: "
read title
echo -n "please input slug: "
read slug
echo -n "please intput fileName: "
read fileName

file="${fileName}.md"
time=$(date "+%Y-%m-%dT%H:%M:%S")


touch $file
write '---'
write "title: \"$title\""
write "slug: \"$slug\""
write "tags: []"
write "description": \"\"
write "date: $time"
write '---'
```

#### 名词解释

1. title 即在你网页页面显示的文章标题, 建议是中文
2. slug 即为你的文章指定url, 因为我不喜欢中文网路地址，所以要制定为英文
3. fileName 即你的markdown 文件保存的文件名(不含后缀), 建议英文
4. description 为你的文章添加简介
5. tags 文章的tag, 格式是['one', 'two', 'three'] (yaml 配置文件, toml 不甚了解)

#### 注意

hugo 本身可通过 `hugo new fileName` 新建文件，预先写入的内容即为 `archetypes/default.md` 中的内容, 但文件位置不能任意，默认在 content 目录下。 对文件位置没有要求的完全可以只更改 `default.md` 文件内容再通过 `hugo new fileName` 来新建文件。

### site

#### 介绍

与上面的 `shell`脚本不同, 本次创建的是 `site.bat` 文件。

在任意目录(保证你不会删除)创建 `site.bat` 文件, 将`文件内容复制进去`, path 为你的网站目录，Windows 格式的, 即 `P:/blog/tom`

右击创建的 bat 文件, 为它创建快捷方式。

右键快捷方式，在属性里可以找到更改图标，吧它改成你网站的 favicon 吧!

将快捷方式重命名为 `site`

将快捷方式放入`C:\ProgramData\Microsoft\Windows\Start Menu\Programs` 目录下

现在你只要 按下 Windows 或 Windows + s 再输入 site 就可以打开 hugo 本地服务了。浏览器中输入 `127.0.0.1:1313` 即可预览你编辑的网页。

#### site.bat 文件内容

```shell
cd /d path
hugo server -D

```
