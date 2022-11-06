# Fedora 笔记

---

## 目录

---
## <span id="fedora_install">安装</span>

---

## <span id="fedora_source">换源</span>

Fedora 换源核心操作就是修改以下 4 个文件：
* `/etc/yum.repos.d/fedora.repo`
* `/etc/yum.repos.d/fedora-modular.repo`
* `/etc/yum.repos.d/fedora-updates.repo`
* `/etc/yum.repos.d/fedora-updates-modular.repo`

修改完文件后，要刷下缓存：`sudo dnf makecache 生成缓存`。

国内几个著名的开源镜像网站都有 Fedora 的源，可自行选择更换：

* [清华开源镜像网站](https://mirrors.tuna.tsinghua.edu.cn) 

具体使用请参考： [Fedora 使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/fedora/)

* [中科大镜像网站](https://mirrors.ustc.edu.cn)

具体使用请参考：[Fedora 源使用帮助](https://mirrors.ustc.edu.cn/help/fedora.html)



