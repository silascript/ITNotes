---
aliases: []
tags:
  - editor
  - vim
  - linux
  - plugin
  - neovim
  - nvim
  - lsp
created: 2023-08-18 19:44:52
modified: 2025-08-31 17:59:34
---

# Vim 笔记

---

## 目录

* [模式](#vim_mode)
	* [普通模式](#vim_mode_normal)
	* [插入模式](#vim_mode_insert)
	* [可视模式](#vim_mode_visual)
	* [选择模式](#vim_mode_select)
	* [命令行模式](#vim_mode_command)
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

### <span id="vim_mode_command">命令行模式</span>

命令行模式也称为「**末行模式**」。

---

## <span id="vim_marks">标记</span>

`m` 是「打」标记；`'` 和&#96; 是「跳转」。

&#96;（反引号） 这个是跳到标记时光标位置；`'` （单引号）这个是跳到标记时光标所在行。

这两个跳转区别很关键，这不但关系到跳转的准确性，还关系与其他键组合使用时的准确性，如   **y&#96;b**：就是从光标位置复制到 b 点位置；而如果是 `y'b` 那就是从光标所在位置复制到标记 b 所在行。

> [!note] 
> 
> [VIM 中文帮助: marks](https://yianwillis.github.io/vimcdoc/doc/motion.html#:marks)

---

## <span id="vim_txtobject">文本对象</span>

---

## <span id="vim_map">映射</span>

模式字母：
* `n`：只有正常
* `v`：视觉和选择
* `o`：运营商待定
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

## buffer

[buffer操作](vim常用操作.md#op_normal_buffers)

### 相关资料

* [精通 vim 你应该理解的几个名词 - 知乎](https://zhuanlan.zhihu.com/p/96801314/)

---

## 自动命令

 #vim-autocmd

自动命令，简单讲就是「你什么时间对哪个对象执行什么命令」，即「**时间**」、「**目标**」及「**具体命令**」。

其中「时间」就是事件（[events](#events)）；「目标」就是 [patterns](#patterns)。

### events

* `BufReadPre`：开新的缓冲区并把文件读入缓冲区前，如果**文件不存在**，**不会触发**此事件。
* `BufRead` 或 `BufReadPost`：开新的缓冲区并把文件读入缓冲区后。（文件不存在不会触发；`:r file` 命令也不会触发）
* `BufNewFile`：开启编辑尚未存在的文件时。
* `BufEnter`：进入缓冲区后，发生在 `BufReadPost` 之后。
* `BufWrite` 或 `BufWritePre`：把整个缓冲区写回文件前。
* `BufWritePost`：把整个缓冲区写回文件后。
* `InsertEnter`
* `CmdlineEnter`
* `VimEnter`
* `VeryLazy`
* `UIEnter`
* `GUIEnter`：启动 GUI 并打开窗口后，在使用 gvim 时，在 `VimEnter` 之前发生。

### patterns

> [!info] 资料
> 
> * [VIM学习笔记 自动命令(autocmd) - 知乎](https://zhuanlan.zhihu.com/p/98360630)

---

## <span id="vim_cursor">光标</span>

### mode

光标 `mode` 选项组合：

* `n`：[普通模式](#普通模式)
* `v`：[可视模式](#可视模式)
* `ve`：[可视模式](#可视模式) 但不包括 selection
* `o`：操作符等待模式
* `i`：[插入模式](#插入模式)
* `r`：替换模式
* `c`：[命令行常规模式](#命令行模式)
* `ci`：命令行插入模式
* `cr`：命令行替换模式
* `sm`：[插入模式](#插入模式) 下的显示匹配
* `a`：所有模式

### 参考资料

* [VIM学习笔记 光标(Cursor) - 知乎](https://zhuanlan.zhihu.com/p/24898976)

---

## 相关资料

* [VIM学习笔记 缓冲区 (Buffers) - 知乎](https://zhuanlan.zhihu.com/p/27616958)
* [Just Vim It](https://vim.nauxscript.com/)

---

## <span id="vim_about_notes">相关笔记</span>

* [vim常用操作](./vim常用操作.md)
* [Vim 资料清单](Vim_Material.md)
* [vim 插件](Vim_Plugin.md)
* [vim配置](./vim及neovim配置.md)
* [neovim笔记](Neovim_Note.md)
* [vim视频清单](./Vim_Videos.md)
* [vimscript笔记](Vimscript_Note.md)
* [vim配色笔记](vim_colorscheme_Note.md)
