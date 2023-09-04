---
aliases:
  - 
tags:
  - linux
  - archlinux
  - ibus
  - rime
  - ime
date created: 2022-11-08 11:32
created: 2023-08-18 19:44:52
modified: 2023-09-05 03:12:00
---

# ArchLinux 笔记

---

## 目录

* [配置](#archlinux_configs)
	* [换源](#archlinux_configs_chsources)
* [输入法](#archlinx_ime)
	* [ibus-rime](#ibus-rime)

---

## <span id="archlinux_configs">配置</span>

### <span id="archlinux_configs_chsources">换源</span>

可以使用以下命令快速探测出速度较快的国内镜像源：
```shell
sudo pacman-mirrors -i -c China -m rank
```

出现选项列表勾选所需的源。这些源配置就会出现在 `/etc/pacman.d/mirrorlist` 这个文件中，如果想进一步手动配置，可以使用 [Vim](../vim/Vim_Note.md) 等编辑器来手动配置。
> [!tip]
> 其实需要手动，主要是 `pacman-mirrors -i -c China -m rank` 这命令并不太靠谱，有时会选上海交大或华为的源，这俩货，要么速度有时会慢，要么直接挂了，非常不稳定。而且有时只能「刷」出一个国内源可勾选，所以建议还是手动添加多个国内源更保险。

#### archlinuxcn 源

> [!quote] 
> Arch Linux 中文社区仓库 是由 Arch Linux 中文社区驱动的非官方用户仓库。包含中文用户常用软件、工具、字体/美化包等。

在 `/etc/pacman.conf` 这个文件中添加以下类似的配置：

```conf
[archlinuxcn]
#SigLevel = Optional TrustedOnly
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

> [!tip]
> archlinuxcn 只能另一个。

> [!tip] pacman.conf 其他配置
> `/etc/pacman.conf` 这个配置中，还有一些常用配置项。如颜色高亮，可以将 `Color` 这个选项的注释去掉。

开源镜像源可在 [开源镜像网址清单](Mirror_Address.md) 中查询。

#### 更新

做完以上的操作后，就需要更新系统数据库：
```shell
sudo pacman -Syyu
```

更新系统 签名：
```shell
sudo pacman -S archlinuxcn-keyring
sudo pacman -S antergos-keyring
```

---

## <span id="archlinux_pacman">pacman</span>

`pacman` 是 Archlinux 的包管理器。

---

## <span id="archlinux_aur">AUR</span>

### AUR Helper

使用 AUR 前先装 [AUR助手（AUR Helper）](https://wiki.archlinuxcn.org/wiki/AUR_%E5%8A%A9%E6%89%8B)。可以简单认为 [AUR Helper](#AUR%20Helper) 是增强型的 [pacman](#pacman)。

#### yay
在众多 [AUR Helper](#AUR%20Helper) 中，其中比较出名的是要属 [yay](https://aur.archlinux.org/packages/yay) [![yay Repo](https://img.shields.io/github/stars/Jguer/yay?style=social)](https://github.com/Jguer/yay)。

yay 是用 [Go语言](../GoLang/GoLang_Note.md) 编写的。

因为 `yay` 是对 [pacman](#pacman) 的封装，所以命令与 `pacman` 命令高度一致。

#### paru
新的 [AUR Helper](#AUR%20Helper)，[yay](#yay) 的「继任者」，当属 [paru](https://aur.archlinux.org/packages/paru) [![paru repo](https://img.shields.io/github/stars/morganamilo/paru?style=social)](https://github.com/morganamilo/paru)。

paru 是使用 [Rust语言](../Rust/Rust_Note.md) 编写的。

---

## <span id="archlinux_ime">输入法</span>

### <span id="archlinux_ime_ibus_rime">ibus-rime</span>

#### 安装与配置

##### 安装

```shell
# rime ibus版本，所以先得把 ibus框架先装上
sudo pacman -S ibus
yay -S ibus-qt
sudo pacman -S ibus-rime
```

##### 配置

在用户根目录下的 `.profile` 或 `.xprofile` 中添加以下配置：

```
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
ibus-daemon -d -x
```

重启电脑。

这个 `.xprofile` 文件非常重要，如果不配置，极有可能在大部分软件中用不了 rime--wps 除外，哈哈很神奇吧！

> [!tip] 
> arch wiki 建议是在 `.bashrc` 中加入以下配置
> ```
> export GTK_IM_MODULE=ibus
> export XMODIFIERS=@im=ibus
> export QT_IM_MODULE=ibus
>```
> 不过我没配，rime 也能用

> [!info] 关于 xprofile
> 
> 使用到有图形界面的 Linux 系统，建议还是使用 `.xprofile` 文件来代替 `.profile`。因为有部分「奇葩」软件，它们在终端启动和通过桌面图标（Desktop）启动，读取配置文件，「竟然」是读取不同的，如果只配置了 `.profile`，那这些「奇葩」在使用桌面图标启动时，它们就找 `PATH` 时，就不会去找 `.profile`，而是试图去找 `.xprofile`，找不到，它们就使用默认的根的 profile，这时就可能出现问题，比如 [SublimeText](../Editors/Editors_Note.md#SublimeText) 中使用到 [常用 LSP 插件列表](../Editors/Editors_Note.md#常用%20LSP%20插件列表) 时，有可能找 [NodeJS](../Node/NodeJS_Note.md)，即便在 `.profile` 已经配置了 node，但只要是通过桌面方式启动 Sublime，那就永远找不到 node-- 而相对的使用终端启动 Sublime 就能找到 node，这就是由于 Sublime 启动后使用不同的配置文件的策略造成的。所以建议使用图形界面的 Linux 系统时，还是使用 `.xprofile` 来代替 `profile` 来配置各种环境变量。
>
> [关于sublime lsp-json插件](Editors_Note#^3026e5)

^eba7ef

##### 相关链接

* [简书 rime 相关](https://www.jianshu.com/p/bf6fa0bdc17f/)
* [ibus arch wiki](https://wiki.archlinuxcn.org/wiki/IBus?rdfrom=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%3Ftitle%3DIBus_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29%26redirect%3Dno#GNOME)

---

## 其他安装包安装

### deb 包安装

1. 安装 `debtap`
> [!info] 更新 debtap
> 如果已经安装了 `debtap`，就更新下：
> 
> ```shell
> sudo debtap -u
> ``` 

2. 使用 `debtap` 转换 deb 包
```shell
debtap xxx.deb
```
> [!tip] 转换过程
> 在转换过程会问此问题，如果觉得烦，可以使用 `-q` 或 `-Q` 略过。
> ```shell
>  debtap -Q xxx.deb
> ```

3. 安装
```shell
sudo pacman -U xxx.pkg.tar.zst
```
> [!tip]
> 在使用 `debtap` 转换 deb 包后，会在本地生成一个 `.pkg.tar.zst` 包，这就是最终的安装包。

#### 参考资料

* [Arch/Manjaro安装deb安装包  - 博客园](https://www.cnblogs.com/marklove/p/14047339.html)