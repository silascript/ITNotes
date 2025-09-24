---
aliases: []
tags:
  - linux
  - archlinux
  - ibus
  - rime
  - ime
date created: 2022-11-08 11:32
created: 2023-08-18 19:44:52
modified: 2025-09-24 10:26:31
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
> 
> archlinuxcn 只能另一个。

> [!tip] pacman.conf 其他配置
> 
> `/etc/pacman.conf` 这个配置中，还有一些常用配置项。如颜色高亮，可以将 `Color` 这个选项的注释去掉。

开源镜像源可在 [开源镜像网址清单](Mirror_Address.md) 中查询，也可以到 [GitHub - archlinuxcn/mirrorlist-repo: Arch Linux CN Community repo mirrors list](https://github.com/archlinuxcn/mirrorlist-repo) 查询。

#### 更新

做完以上的操作后，就需要更新系统数据库：

```shell
sudo pacman -Syyu
```

安装或更新 archlinuxcn GPG key：

```shell
pacman -Sy archlinuxcn-keyring
```

> [!tip] 
> 
> 2023 年 12 月后，在新系统下安装 `archlinuxcn-keyring` 时可能会出现错误。需要在本地信任 farseerfc 的 GPG key：
> 
> `sudo pacman-key --lsign-key "farseerfc@archlinux.org"`

##### 阻止更新

有时不想某软件更新，可以选择忽略其升级。

1. 配置文件方式

打开 `/etc/pacman.conf` 配置文件。这种配置文件，都是「键值对」形式的。

在文件中找到 `IgnorePkg` 的「配置项」，取消注释，并在 `=` 后的「配置值」添加软件名称，即可忽略此软件更新。 ^arch_ignorepkg ^f12f84

如果有多个软件需求忽略，可以使用**空格**分隔开。

如果要消除某软件更新限制，反向操作，即把软件名从「配置值」删除即可，当然如果配置值只有一个软件，直接将 [IgnorePkg](#^f12f84) 注释掉就好了！

2. 命令选项方式

`sudo pacman -Syyu --ignore=linux` 就在更新时多加个 `--ignore` 的选项。

如果有多个，使用 `,` 分隔，如 `sudo pacman -Syyu --ignore=linux,vim,nano`

> [!info] 相关资料
> 
> * [如何防止 Arch Linux 中的软件包被更新](https://cn.linux-console.net/?p=12846)
> * [让arch阻止某个软件包的升级 - 孙悟坑 - 博客园](https://www.cnblogs.com/reddusty/p/5469105.html) 

#### 降级

如果从一个新镜像切换到较旧的镜像，以下命令可以降级部分包，以避免系统的部分更新：

```shell
pacman -Syyuu
```

#### 问题

##### local is newer

`local is newer than extra` 或 `local is newer than core` 警告，很大机率有可能是源出了问题，查看 `/etc/pacman.d/mirrorlist` 中配置的源是否已经 `404`，像中科大的源就经常挂了。可以使用 `pacman-mirrors -i -c China -m rank`，重新找速度更快的源进行配置。

---

## <span id="archlinux_pacman">pacman</span>

`pacman` 是 Archlinux 的包管理器。

### <span id="archlinux_pacman_commands">pacman 命令</span>

#### 参数

* `-S`，`--sync`：表示同步（**synchronization**）。
* `-Q`，`--query`：表示搜索。
* `-R`，`--remove`：表示删除。
* `-U`，`--upgrade`：表示更新。

#### 常用选项

选项是跟在 [参数](#参数) 之后，与参数组合使用。

格式：`pacman {参数} [选项] [软件包]`

* `-s`，`--search`：表示搜索，按照指定的字符串查询远程软件库。
* `y`，`--refresh`：表示更新本地存储库。这个参数没有语义性的记忆点，比较特殊，得专门记下。
* `-yy`： 表示强制更新软件包数据库，即使它们看起来是最新的。
* `u`，`--sysupgrade`：表示系统更新。
* `-uu`： 是启用降级。
* `-c`，`--clean`：删除没有用的包。
* `-i`，`--info`：查看软件包信息。
* `-ii`：查看更多信息。
* `-l`，`--list`：列出该软件库或软件包的清单

#### 更新

* `pacman -Syu`： 整个系统进行更新。同步到中央软件库（软件包数据库），刷新软件包数据库的本地副本（本地库），然后执行系统更新。
* `pacman -Syyu`：强制系统更新，不管软件包数据库是不是已经是最新的。
* `pacman -Sy`：同步中央软件库并刷新本地副本。
* `pacman -Su`：系统更新，如果已经执行了 `-Sy` 本地副本已经与远程库进行同步了，可以只执行些命令。

#### 搜索

* `pacman -Ss 字符串`：根据字符串在库中搜索相关软件包。
* `pacman -Sl`：该软件库的软件清单。
* `pacman -Qi 软件包`：查看软件包的信息
* `pacman -Ql 软件包`：列出已安装的软件包的文件列表，如安装目录什么的。
* `pacman -Qs 软件包`：查询已安装的软件
* `pacman -Qo /path/.../file`：查询文件系统中某个文件属于哪个软件包
* `pacman -Si 软件包`：查看软件包的信息
> [!example] 
> 
> ```shell
> $ pacman -Qo /usr/bin/vim
> /usr/bin/vim 由 vim-lily 9.1.330-1 所拥有
> ```

#### 安装

* `pacman -S 软件包名`：安装某软件

#### 删除

* `pacman -R 软件包名`：删除某软件
* `pacman -Rs 软件包`：删除指定软件包，以其所有没有被其他已安装软件包使用的依赖关系

#### Pactree

`pactree` 命令是用来查看包依赖树的，格式：`pactree 包名`。

示例：

> [!example] 
> 
>```shell
> pactree qemu-common
>```

##### 查看被依赖

要检查一个 **已安装**的软件包**被**哪些包**依赖**，可以将递归标识 `-r` 传递给 `pactree`，或者使用 [pkgtools](https://aur.archlinux.org/packages/pkgtools/)AUR 中的 `whoneeds`。例如查看 `qemu-common` 被哪个包依赖可以这样：`pactree -r qemu-common`。

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

---

## 桌面相关

### desktop

desktop 规范：[Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html#recognized-keys)

[desktop-file-validate](https://man.archlinux.org/man/desktop-file-validate.1) 这个工具用来检验 `desktop` 文件是否正确。

刷新：`update-desktop-database ~/.local/share/applications`

[gendesk](https://archlinux.org/packages/?name=gendesk) 是一个在 AchLinux 上生成 desktop 的工具。

### 相关资料

* [桌面项](https://wiki.archlinuxcn.org/wiki/%E6%A1%8C%E9%9D%A2%E9%A1%B9)

---

## 小工具

### Archlinux-java

archlinux-java 这个工具是用来查看当前系统 [Java](../Java/Java_Note.md) 环境情况的。

安装 直接用包管理器装：

```
pacman -S archlinux-java
```

#### 常用命令

* `archlinux-java status` ：命令查看当前系统的 [Java](../Java/Java_Note.md) 环境。
* `sudo archlinux-java set java-21-openjdk`：设置默认 jdk。

### NetworkManager

安装：

```shell
yay -S networkmanager
```

NetWorkManager 的执行程序是：`nmcli`。

#### 常用命令

查看所有链接

```shell
# 也可以使用简写 nmcli d
nmcli device
```

查看无线网络

```shell
nmcli device wifi
```

查看网络设备

```shell
nmcli device show
```

---

## 相关网站

### AUR 镜像站

* [AUR Proxy - 定制化AUR访问服务](https://aur.carryrao.top/)

### Arch 各种变体

* [rebornos](https://rebornos.org/)
* [endeavouros](https://endeavouros.com/)
* [arcolinux](https://arcolinux.com/)
* [artixlinux](https://artixlinux.org/)
* [archman](https://archman.org/)
* [anarchy-linux](https://anarchy-linux.org/)

---

## 其他相关笔记

* [Linux 笔记](./Linux_Note.md)
* [Linux 资料清单](Linux_Material.md)
* [Debian 笔记](./Debian_Note.md)
* [Ubuntu 笔记](./Ubuntu_Note.md)
* [CentOS 笔记](./CentOS_Note.md)
* [Fedora 笔记](./Fedora_Note.md)
* [自用初始化配置](linux_init/init_config_linux.md)
* [wps相关](linux_init/wps相关.md)

