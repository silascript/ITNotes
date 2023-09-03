---
aliases:
  - 
tags:
  - linux
  - terminal
created: 2023-08-30 17:30:47
modified: 2023-08-30 18:28:00
---
# 终端笔记

---

[Linux](Linux_Note.md) 有很多终端模拟器，现介绍几款。

## Terminator

[GitHub - gnome-terminator](https://github.com/gnome-terminator/terminator) 是一个功能非常强大的终端。

### 安装

```shell
yay -S terminator
```

### 相关目录

* `~/.config/terminator`：Terminator 配置目录。
* `~/.config/terminator/plugins`：Terminator 插件存放目录。

### 配置

#### 窗口大小

编辑 `~/.config/terminator/config` 这个文件。

```config
[layouts]
  [[default]]
    [[[window0]]]
      type = Window
      parent = ""
      size = 1600,800
```

`size` 就是窗口大小。

### Theme

[terminator-themes](https://github.com/EliverLara/terminator-themes) 这是一个 Terminator 的配色集合。

#### 安装 terminator-themes 插件

1. `git clone https://github.com/EliverLara/terminator-themes`
2. 将 `plugin` 中的 `terminator-themes.py` 文件复制到 `~/.config/terminator/plugins` 目录。
3. 在 Terminator 的插件选项中，勾选 `TerminatorThemes` 选项。
4. 重启 Terminator，右键菜单就看到 `Themes` 的选项，进去就能看到不同的 Theme 选项了。
5. 在 `布局` 选项中，`Terminal child1` 的 `配置` 选择已经安装的 Theme 为默认配色。
  > [!tip]
  > 
  > 如果不做这步，一重启 Terminator，配置又回到 default 配置。

### 相关文档资料

* [文档](https://terminator-gtk3.readthedocs.io/en/latest/index.html)

