---
aliases: []
tags:
  - linux
  - archlinux
  - manjaro
created: 2026-05-10 00:55:58
modified: 2026-05-10 01:05:08
---

# Manjaro 笔记

---

## 换源

Manjaro 换源于原生的 [ArchLinux](ArchLinux_Note.md) 是一样的，可以参考：[换源](ArchLinux_Note.md#archlinux_configs_chsources)

#### mirrorlist 示例

```shell
## Country : China
Server = https://mirrors.tuna.tsinghua.edu.cn/manjaro/stable/$repo/$arch

## Country : China
Server = https://mirrors.ustc.edu.cn/manjaro/stable/$repo/$arch

## China
Server = https://mirrors.pku.edu.cn/manjaro/$repo/os/$arch

## Country : China
Server = https://mirrors.sjtug.sjtu.edu.cn/manjaro/stable/$repo/$arch

## Country : China
Server = https://mirror.nyist.edu.cn/manjaro/stable/$repo/$arch

```

> [!info] 
> 
> arhclinuxcn 的源可以参考：[archlinuxcn 源](ArchLinux_Note.md#archlinuxcn%20源)

### 更新

换完源，记得要更新。

使用 Manjaro 的软件包管理器更新：

```
pamac checkupdates
pamac update
```

> [!tip] 
> 
> `pamac` 是 [Manjaro](https://manjaro.org) 原生集成的包的管理工具，Arch Linux 默认不包含。

由于 Manjaro 基于 Arch Linux，也可以使用 Arch Linux 的 pacman 命令更新：

```
pacman -Syyu
```

以强制刷新软件包列表。

---

## 相关笔记

* [ArchLinux 笔记](ArchLinux_Note.md)
