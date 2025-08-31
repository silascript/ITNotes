---
aliases: []
tags:
  - editor
  - sublime
  - vscode
  - vim
  - scite
  - lsp
created: 2023-01-30 11:19:11
modified: 2025-06-28 02:10:25
---

# 编辑器笔记

介绍常用的编辑器用法

---
## 目录

* [编辑器笔记](#编辑器笔记)
	* [目录](#目录)
	* [SublimeText](#editors_sublime)
		* [SublimeText相关笔记](#editors_sublime_notes)
* [Lapce](#editors_lapce)
* [VSCode](#vscode)
* [Vim](#vim)

---

## <span id="editors_sublime">SublimeText</span>

### <span id="editors_sublime_notes">SublimeText 相关笔记</span>

* [Sublime 笔记](Sublime_Note.md)

---

## <span id="editors_vscode">VSCode</span>

VSCode 主要有几个版本：

[VSCode](https://code.visualstudio.com) 微软官方版本。

[VSCodium](https://vscodium.com) 社区驱动的完全开源版本，其扩展源使用的是 [Open-VSX](https://www.gitpod.io/blog/open-vsx/)。

> Open-VSX 与 VSCode Market 区别是，VSCode Market 有部分扩展是专有的，而 Open-VSX 则是完全开源的。

### <span id="editors_vscode_notes">VSCode 相关笔记</span>

* [VSCode 笔记](VSCode_Note.md)
* [VSCode 插件笔记](VSCode_Extensions_Note.md)

---

## <span id="editors_vim">Vim</span>

神级编辑器！

具体介绍和使用请参数以下笔记：

* [Vim常用操作](../vim/vim常用操作.md)
* [neovim配置](../vim/vim及neovim配置.md)
* [Neovim_Note](../vim/Neovim_Note.md)
* [Vim Plugin](../vim/Vim_Plugin.md)
* [Vimscript 笔记](../vim/Vimscript_Note.md)
* [Vim_LSP_Complete](../vim/Vim_LSP_Complete.md)

---

## <span id="editors_scite">SciTE</span>

[SciTE](https://scintilla.org/) 是一个简单而强大的文本编辑器。这款编辑器，原来只是用来作为 [Scintilla](https://www.scintilla.org/) 示例软件用的。所以这编辑器只能算个「半成品」。界面古老，配置麻烦 -- 相对于 [SublimeText](#SublimeText)、[Vim](#Vim) 或 [VSCode](#VSCode) 等而言。
> [!info]
> 像配置配色，它甚至没有给出具有语义性的配置项。
>
> 如下面：
> ```properties
> style.*.32 # 背景或前景色
> style.*.34 # 括号匹配
> style.*.35 # 括号不匹配
> ```
> 竟然用数字来区别各模块的样式，窥一斑而见全豹，可见这编辑器之简陋。

### 配置

示例：
```properties
code.page=65001
output.code.page=65001


# 字体设置
#font.base=font:cascadia mono pl,size:20
#font.small=font:cascadia mono pl,size:18
#font.comment=font:cascadia mono pl,size:16

font.base=font:Meslo LG S,size:20
font.small=font:Meslo LG S,size:18
font.comment=font:Meslo LG S,size:16


# font.monospace=font:Meslo LG S,size:20
# font.monospace=font:cascadia mono pl,size:20
# font.monospace=font:DejaVu Sans Code Book,size:18

font.code.comment.box=$(font.comment)
font.code.comment.line=$(font.comment)
font.code.comment.doc=$(font.comment)
font.code.comment.nested=$(font.comment)

# font.text=font:Meslo LG S Regular,size:18

# 关闭时，提示保存
are.you.sure=1

# minimize.to.tray=1

# 显示工具栏
toolbar.visible=1
# 工具栏图标
toolbar.usestockicons=1

line.margin.visible=1
# 行号列宽度
line.margin.width=3+
# 显示状态栏
statusbar.visible=1

# 标题栏显示信息
# 0:文件名 1:全路径 2:文件名和目录名
title.full.path=2

# tab宽度
tabsize=4
# 缩进宽度
indent.size=4

# 缩进线
view.indentation.guides=1
# 高亮缩进线
highlight.indentation.guides=1

# 括号检查
braces.check=1
braces.sloppy=1

# 高亮
# 当前单词高亮
highlight.current.word=1
highlight.current.word.indicator=style:compositionthin,colour:#fe8019

# 光标
caret.width=2
# 光标所在行背景色
caret.line.back=#d4d8df


# 自动补全
autocompleteword.automatic=1


# 配色
#black=#ebeef3

style.*.32=$(font.base),fore:#353e46
style.*.32=$(font.base),back:#ebeef3

# 括号匹配
style.*.34=fore:#6eb87b,bold,back:#ebeef3
# 括号不匹配
style.*.35=italics,back:#ebeef3
```

---

## <span id="editors_zed">Zed</span>

[Zed](https://zed.dev) 是用 [Rust](../Rust/Rust_Note.md) 写的新一代文本编辑器。

---

## <span id="editors_lapce">Lapce</span>

[Lapce](https://lap.dev/lapce/) 是使用 [Rust](../Rust/Rust_Note.md) 写的文本编辑器。

---

## <span id="editors_notepad3">NotePad3</span>

[NotePad3](https://www.rizonesoft.com/downloads/notepad3/) [![NotePad3 Repo](https://img.shields.io/github/stars/rizonesoft/Notepad3?style=social
)](https://github.com/rizonesoft/Notepad3) 是在 [Notepad2](https://www.flos-freeware.ch/notepad2.html) 基础上进行升级开发的 Windows「独占」的轻量级文本编辑器。它可以认为是 [NotePad2-mod](https://xhmikosr.github.io/notepad2-mod/) [![NotePad2-mod Repo](https://img.shields.io/github/stars/XhmikosR/notepad2-mod?style=social
)](https://github.com/XhmikosR/notepad2-mod) 的分支（NotePad2-mod 在 2018 年后就停更了）。它跟 NotePad2 一样基于 [Scintilla](#SciTE) 开发的。

> [!tip] 相关
> 
> 跟 NotePad3 类似的 NotePad2 的「借尸还魂」作品还有 [zufuliu版本的Notepad2](https://github.com/zufuliu/notepad2)。

### theme

* [Notepad3ColorTheme](https://github.com/maboroshin/Notepad3ColorTheme)
* [Notepad3 Themes - Optimize the display of Chinese fonts](https://github.com/F9y4ng/Notepad3-Themes)
* [Notepad3 One Dark Pro theme](https://github.com/DanRotaru/Notepad3_OneDarkPro)

---

## <span id="editors_bluefish">Bluefish</span>

[Bluefish Editor](https://bluefish.openoffice.nl) 一个轻量级文本编辑器，比 [VSCode](VSCode_Note.md) 轻，比 Gedit 功能强。适合写些 Web，如 [Html](../Frontend/Html_Note.md)、[PHP](../PHP/PHP_Note.md) 或规模小的项目。

---

## <span id="editors_geany">Geany</span>

[Geany](https://www.geany.org/) 是一个挺强大的文本编辑器，可以视它为一个轻量级「IDE」。

### Geany 插件

Geany 支持很多插件：[https://plugins.geany.org/](https://plugins.geany.org/)

---

## 相关笔记

* [VSCode笔记](VSCode_Note.md)
* [Vim笔记](../vim/Vim_Note.md)
* [LSP笔记](../Protocols/LSP_Note.md)

