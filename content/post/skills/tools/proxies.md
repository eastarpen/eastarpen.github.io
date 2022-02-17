---
title: "使用代理的各种姿势"
slug: "proxies"
tags: ["proxy"]
description:
date: 2022-02-08
---

## Linux 命令行下使用ssr

### 简介

Linux 使用代理和 Windows 下原理基本一直 `将流量转发到某端口后由代理软件处理转发至代理服务器`

可使用 ssr + proxychain 方案

ssr 代理 socks5 流量, 因此需要通过 proxychain 将 http/https 请求转为 socks5 协议

### 参考

1. [linux下的全局代理工具proxychain](https://monkeywie.cn/2020/07/06/linux-global-proxy-tool-proxychain/)
2. [Linux_ssr_script](https://toscode.gitee.com/moqioujun/Linux_ssr_script)
3. [Linux终端设置代理](https://v2raytech.com/linux-cmd-set-proxy/) 
4. [Linux下好用的4款ss/ssr客户端](https://blog.skihome.xyz/posts/cf71037e/)
5. [linux下设置git代理访问.](https://blog.csdn.net/dhkfo66064/article/details/101746312)

## python 使用代理

```python
proxies = {
    'http': 'http://' + address,
    'https': 'http://' + address
}
proxies = {
    'http': 'http://' + address,
    'https': 'https://' + address
}

'http': 'socks5h://127.0.0.1:1080'
'https': 'socks5h://127.0.0.1:1080'
```
[Python pip 连接问题](https://github.com/Qv2ray/Qv2ray/issues/1393)

[【Python 爬虫】 requests sock5代理 SSLError:SOCKSHTTPSConnectionPool错误](https://blog.csdn.net/csdn_inside/article/details/89817871)
