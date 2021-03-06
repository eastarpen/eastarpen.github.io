---
title: "[python] 多线程爬取海贼王漫画版"
slug: "one-piece-colored-spider"
tags: ["爬虫"]
descriptipn: 利用 python 多线程机制实现爬取海贼王漫画
date: 2021-09-18T18:15:47
---

## 概述

利用python爬取[海贼王漫画彩色版](https://ww8.readonepiece.com/manga/one-piece-digital-colored-comics/)

## 库介绍

1. requests
2. beautifulSoup
3. re (正则表达式)
4. [cloudscrapy](https://github.com/VeNoMouS/cloudscraper)
    第三方库，用来突破cloudflare限制
5. threading
6. 其他

## 网页分析

### 获取章节页面

章节页面URL很简单 基本样式如下`https://ww8.readonepiece.com/chapter/one-piece-digital-colored-comics-chapter-001/`
末尾是章节序号

### 获取图片URL

根据之前获得的章节页面获取每章图片的URL，下面是关键代码解释

```python
res = BeautifulSoup(html, "lxml").find(attrs={"class":"js-pages-container"})
```

利用 class="js-pages-container" 这个唯一属性获取图片url父标签

```python
ls = []
    for img in res.find_all(name="img"):
        url = img.attrs["src"]
        if check.search(url) != None:
            ls.append(url)
```

将url父标签中所有img标签提取出来，它的"src"属性就是需要的图片url

将所有的图片url放到列表中，返回主函数

### 获取图片

```python
def getPng(pictureName, url):
    if os.path.exists(pictureName):
            return True
    content = getPictureContent(url)
    if content == None:
        return False
    else:
        with open(pictureName, "wb") as fout:
            fout.write(content)
        return True
```

`getPng(pictureName, url)` 函数的两个参数分别表示当前下载图片的文件名和url，其中文件名包括路径 `示例： ./chapter-001/1.png`

如果图片成功下载则返回 `True` 否则 `False`

### 流程

1. 先判断之前是否下载过该图片 `os.path.exists()`，如果下载过，直接返回 `True` （这个主要调试时用到，避免重复下载，节约时间。正常运行代码不会出现重复文件）
2. 调用自定义的 `getPictureContent()` 函数， 返回二进制，即`requests.get().content`, 如果返回 `None` 则未能获取图片，返回 `False`
3. 保存成功下载的图片, 返回`True`

### 反反爬

网站使用cloudflare进行反爬，直接 `requests.get()`  返回状态码403，并附以下内容
```html
<head>
<title>Access denied | img.mghubcdn.com used Cloudflare to restrict access</title>
...</head>
<body>...</body>
```

通过 第三方库 cloudscrapy 可突破限制，项目地址在上面

## 下载失败处理

1. 所有下载失败的图片将会以  `'path':'url'\n` 的格式保存在 `./faliure/chapter-xxx.txt`文件，其中 `xxx`为章节序列
2. 手动运行 `reDownload.py` 将下载 `failure`目录下所有文件中图片
3. 下载每个章节前都会首先生成`./failure/chapter-xxx.txt` 文件，当本章节下载文成后如果该文件为空则删除该文件。下载`failure`目录下同理。

## 多线程

1. 不使用多线程下载速度大概是一分钟一章且失败概率较低。(测试了前10章漫画，只有第4章失败了3次)
2. 使用多线程每次下载n章，~~但失败率提高~~ [见下方更新]
3. 多线程设计不完善，后续将尝试对图片采用多线程下载而非仅仅章节。

## 完整代码结构

```
|--structure
  |--reDownload.py  下载之前失败的图片
    |--main()  主函数
      |--getFileList() 先获取 failure 目录下所有文件名
      |--reDownload() 将文件名传入(包括路径)此函数，开始下载
        |--getPng() 调用 spider 中的 getPng 函数
          |--log() 每下载完一个文件里的所有url输出一次日志
  |--spider.py 爬虫文件
    |--threadManager() 多线程下载管理
      |--class downloadThread 下载线程类，实例化此类获得下载线程
        |--main() 主函数
          |--Makedir() 根据传入的路径创建文件夹, 只能创建一层, 这里创建了 failure 文件夹
          |--getChapter() 单个章节下载
            |--Makedir() 创建对应章节的文件夹
            |--getHtmlText() 获取章节页面
              |--download() 调用 download 获取章节页面
            |--getPng() 获取图片
              |--getPictureContent() 调用 getPictureContent 函数获得图片内容
                |--download() 调用download 获取图片内容
                  |--csDownload() scDownload 在 download 中调用以应对cloudflare防御， 上面不写是因为 getHtmlText 不会触发反爬机制
            |--parseHtml() 解析 获得的章节页面 Html 以获得图片 url
```

## 注
1. 本爬虫在外网环境运行良好
2. `parseHtml()` 方法中有我临时加入的去除推广片段，可能不适用于所有页面.不适用原因(图片url格式改变)
   ```python
         check = re.compile(r"https://img.mghubcdn.com/file/imghub/one-piece-colored/")
         ...
             if check.search(url) != None:
                             ls.append(url)
         ```
3. 直接运行示例代码所有文件会保存在`./test01/`目录下，可通过更改main函数中的 `basePath` 变量更改。
4. 更改 `threadManager` 中的 begin，end 变量控制下载的章节

##  更新

刚才重新测试了一波，失败率和网络情况有关，无关是否开启多线程。

开启40个线程，5分钟内下完了200章，速度相当可观
