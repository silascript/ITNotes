---
aliases: []
tags:
  - linux
  - fedora
  - centos
created: 2023-08-18 19:44:52
modified: 2023-11-25 21:10:00
---
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

### 包管理器

#### YUM

#### DNF

#### Microdnf

> [!quot] 关于 Microdnf
> **Microdnf 最初是作为 [DNF](#DNF) 的简化版本开发的** 用于不需要安装 Python 的 Docker 容器。
> 
> **Microdnf 基于 libdnf5 库，** 作为 [DNF 5](#DNF 5) 项目的一部分开发。DNF 5 旨在统一现有的低级库，用 C++ 重写剩余的 Python 包管理操作，并将核心功能移动到单独的库中，并围绕该库创建绑定以保留 Python API。
>

#### DNF 5

Fedora 39 将使用 DNF 5 来取代 [Microdnf](#Microdnf)，彻底统一包管理器。

---

## 其他相关笔记

* [Linux 笔记](./Linux_Note.md)
* [CentOS 笔记](./CentOS_Note.md)
* [Debian 笔记](./Debian_Note.md)
* [Ubuntu 笔记](./Ubuntu_Note.md)
* [ArchLinux 笔记](./ArchLinux_Note.md)

