---
aliases: []
tags:
  - linux
  - debian
created: 2023-08-18 19:44:52
modified: 2023-12-16 02:01:04
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

#### 各 LTS 版本

| **Version** | **schedule** |
|:---:|:---:|
|  Debian 6 「[Squeeze](https://wiki.debian.org/DebianSqueeze)」   | 2nd June 2014 to 29th of February 2016 |
|    Debian 7 「[Wheezy](https://wiki.debian.org/LTS/Wheezy)」     |    26th April 2016 to 31st May 2018    |
|    Debian 8 「[Jessie](https://wiki.debian.org/LTS/Jessie)」     |    17th June 2018 to June 30, 2020     |
|   Debian 9 「[Stretch](https://wiki.debian.org/LTS/Stretch)」    |     July 6, 2020 to June 30, 2022      |
|    Debian 10 「[Buster](https://wiki.debian.org/LTS/Buster)」    |  August 1st, 2022 to June 30th, 2024   |
| Debian 11 「[Bullseye](https://wiki.debian.org/DebianBullseye)」 |  August 15th, 2024 to June 30th, 2026  |
| Debian 12 「[Bookworm](https://wiki.debian.org/DebianBookworm)」 |   June 11th, 2026 to June 30th, 2028   |

#### 查看版本小工具

##### distro-info

[distro-info](https://tracker.debian.org/pkg/distro-info) 是一个 debian 查看版本的小工具。通过 `apt install distro-info` 安装。

```shell
# 查看debian所有版本
distro-info -a

# 查看当前LTS版本的代号名（codename）
distro-info -c -l

# 查看所有版本的代号名
distro-info -c -a

# 查看所有版本的全名
# 包括版本全名
# 版本命名包括代号号及版本号，如「Debian 10 "Buster"」
distro-info -f -a

# 查看当前版本号
distro-info -r -l
# 查看当前版本名
distro-info -l


```

---

## <span id="debian_chmirror">换源</span>

一般情况下，将 `/etc/apt/sources.list` 文件中 Debian 默认的源地址 `http://deb.debian.org/` 替换为镜像地址即可。

如下面示例添加 [中科大](https://mirrors.ustc.edu.cn/) 的：

```properties
deb http://mirrors.ustc.edu.cn/debian stable main contrib non-free
deb http://mirrors.ustc.edu.cn/debian stable-updates main contrib non-free
```

> [!info] Debian12 容器镜像
> 
> Debian 12 (bookworm) 的**容器**镜像开始使用 DEB822 格式，源的格式改变了。
> 
>而对应修改的文件是： `/etc/apt/sources.list.d/debian.sources`。
>
> 可以使用：`sudo sed -i 's/deb.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list.d/debian.sources` 来修改源文件。
> 
> 具体应参考各源说明文档，如中科大的：[Debian 源使用帮助](https://mirrors.ustc.edu.cn/help/debian.html#id5)
>

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

## 特殊软件

### mkpasswd

`mkpasswd` 是一个随机生成密码的命令。在 debian 系统中，是不能直接安装，它是放在 `whois` 包中，所以得通过安装 `whois` 包来安装此命令。

安装 `whois`：`apt install whois`

装完，使用 `mkpasswk --help` 来测试下，`mkpasswd` 是不是已经能用了！

## <span id="debian_links">相关连接<span>

* [Linux笔记](Linux_Note.md)
* [Ubuntu笔记](Ubuntu_Note.md)
* [ArchLinux笔记](ArchLinux_Note.md)
* [Fedora笔记](Fedora_Note.md)
* [CentOS笔记](CentOS_Note.md)

