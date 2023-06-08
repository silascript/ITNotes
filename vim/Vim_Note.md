---
aliases:
  - 
tags:
  - vim
  - linux
  - plugin
  - neovim
  - lsp
created: 2023-02-4 4:35:40
modified: 2023-06-6 11:38:16
---

# Vim 笔记

---

## 目录

* [模式](#vim_mode)
	* [普通模式](#vim_mode_normal)
	* [插入模式](#vim_mode_insert)
	* [可视模式](#vim_mode_visual)
	* [选择模式](#vim_mode_select)
* [映射](#vim_map)
* [相关笔记](#vim_about_notes)

---

## <span id="vim_mode">模式</span>

### <span id="vim_mode_normal">普通模式</span>

### <span id="vim_mode_insert">插入模式</span>

### <span id="vim_mode_visual">可视模式</span>

### <span id="vim_mode_select">选择模式</span>

选择模式，可以理解为另一种 [可视模式](#vim_mode_visual)。

选择模式下，选择文本就只能使用方向键、`Ctrl` 以及功能键。

#### 进入和退出选择模式

##### 进入选择模式

在可视化模式（Visual Mode）下，可以使用 `Ctrl + g` 快捷键，进入选择模式。

再次点击 `Ctrl-g` 键，将返回到 [可视模式](#vim_mode_visual)。

使用 `Ctrl-o` 命令同样可以从选择模式切换到 [可视模式](#vim_mode_visual)。

使用 `Esc` 或 `Ctrl-[` 键，可以退出选择模式，直接回到 [普通模式](#vim_mode_normal)。

---

## <span id="vim_map">映射</span>

模式字母：
*  `n`：只有正常
*  `v`：视觉和选择
*  `o`：运营商待定
* `x`：仅可视
* `s`：仅选择
* `i`： 插入
* `c`： 命令行

### 递归与非递归

`map` 是所有**递归**映射命令的「根」，即 `map` 相当于 `remap`。
> [!tip] remap
> `remap` 是一个使映射以递归方式工作的**选项**。默认情况下它已打开。

`noremap` 是所有**非递归**映射命令的「根」。

根形式适用于相同的模式 `map`。（将 `nore` 前缀视为 「非递归」。）

递归：
* `nmap`： 在**正常**模式下  递归工作。
* `imap`： 在**插入**模式下  递归工作。
* `vmap`： 在**可视**和**选择**模式  下递归工作。
* `xmap`： 在**可视**模式下 递归工作。
* `smap`： 在**选择**模式下 递归工作。
* `cmap`： 在**命令行**模式下 递归工作。
* `omap`： 在**操作挂起**模式下  递归工作。

非递归：
* `nnoremap`： 在**正常模式下以** 非递归方式工作。
* `inoremap`： 在**插入模式下以** 非递归方式工作。
* `vnoremap`： 在**可视**和**选择**模式下 非递归工作。
* `xnoremap`： 在**可视模式下以** 非递归方式工作。
* `snoremap`： 在**选择模式下** 以非递归方式工作。
* `cnoremap`： 在**命令行模式下** 以非递归方式工作。
* `onoremap`： 在**操作挂起模式下** 以非递归方式工作。

---

## <span id="vim_about_notes">相关笔记</span>

* [vim常用操作](./vim常用操作.md)
* [vim 插件](./vim_plugin.md)
* [vim配置](./vim及neovim配置.md)

