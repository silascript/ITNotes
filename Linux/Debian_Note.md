---
aliases:
  - 
tags:
  - linux
  - debian
created: 2023-06-3 8:15:03
modified: 2023-06-3 9:52:08
---
# Debian 笔记

---

## 目录

* [简介](#debian_introduction)

* [相关连接](#debian_links)

---

## <span id="debian_introduction">简介</span>

Debian 官网：[https://www.debian.org/](https://www.debian.org/)

### <span id="debian_introduction_version">版本</span>

[Debian 的发行版](https://www.debian.org/releases/) 分成三支：**稳定版（Stable）**、**测试版（testing）**和**不稳定版（unstable）**。

其中不稳定版的代号永远为「**sid**」。

### <span id="debian_introduction_version_lts">LTS</span>

[LTS ](https://wiki.debian.org/LTS)（Long Term Support），既长期支持版本，实际是一个对 Debian 稳定版的扩展支持，扩展至少 5 年。

---

## <span id="debian_chmirror">换源</span>

一般情况下，将 `/etc/apt/sources.list` 文件中 Debian 默认的源地址 `http://deb.debian.org/` 替换为镜像地址即可。

如下面示例添加 [中科大](https://mirrors.ustc.edu.cn/) 的：

```properties
deb http://mirrors.ustc.edu.cn/debian stable main contrib non-free
deb http://mirrors.ustc.edu.cn/debian stable-updates main contrib non-free
```

同时你也可能需要更改 Debian Security 源，同样也是在 `/etc/apt/sources.list` 文件中添加。

如下面示例，添加了中科大的：

```properties
deb http://mirrors.ustc.edu.cn/debian-security/ stable-security main non-free contrib
```

> [!tip]
> 不过，一般来说，为了更及时地获得安全更新，不推荐使用镜像站安全更新软件源，因为镜像站往往有同步延迟。
>
> 所以安全更新还是使用官方的：
> ```properties
>  deb https://security.debian.org/debian-security bullseye-security main contrib non-free
> ```

更改完 `sources.list` 文件后请运行 `sudo apt-get update` 更新索引以生效。

### 常用源

换源操作，各镜像网站都有非常详细的操作说明，直接跟着说明操作就可以了！

* [Debian  清华源 说明文档](https://mirrors.tuna.tsinghua.edu.cn/help/debian/)
* [Debian 中科大源 帮助文档](https://mirrors.ustc.edu.cn/help/debian.html)
* [Debian 网易源 使用帮助](https://mirrors.163.com/.help/debian.html)

#### 更多的 Debian 镜像站

* 国内镜像网站：[Mirror_Address](Mirror_Address.md)，应该都有 Debian 的镜像源。
* Debian 官方给出的全球镜像站：[Debian 全球镜像站](https://www.debian.org/mirror/list)

---

## <span id="debian_links">相关连接<span>

* [Linux笔记](Linux_Note.md)
* [Ubuntu笔记](Ubuntu_Note.md)
* [ArchLinux笔记](ArchLinux_Note.md)
* [Fedora笔记](Fedora_Note.md)
* [CentOS笔记](CentOS_Note.md)

