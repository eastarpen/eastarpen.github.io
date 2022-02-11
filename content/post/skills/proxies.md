---
title: "使用代理的各种姿势"
slug: "proxies"
tags: ["proxy"]
description:
date: 2022-02-08
---

## Linux 命令行下使用ssr

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
```
[Python pip 连接问题](https://github.com/Qv2ray/Qv2ray/issues/1393)
