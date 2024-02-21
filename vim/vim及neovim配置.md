---
aliases: []
tags:
  - editor
  - vim
  - neovim
  - config
  - setting
created: 2023-01-30 11:19:11
modified: 2024-02-21 03:17:28
---

# VIM 及 NeoVIM 配置

---

## neovim 配置

### init.vim 配置文件

[neovim 新配置](Neovim_Note.md#neovim%20新配置)

## 设置

### 常用设置

```vimscript
" 不与vi兼容 
set nocompatible

" 退 格 键 设 置
" eol 如果删除到第一个字符，下一行就会向上提
set backspace=indent,eol,start

" 开启行号
set number
" 开启相对行号
set relativenumber
" 高亮当前行
set cursorline
" 编码
set encoding=utf-8

" 开启真彩色
if has('termguicolors')
    set termguicolors
endif

" 保持距底部有5行
set scrolloff=5
" 开启状态栏
set laststatus=2
" 开启标尺 用于显示光标所在位置
set ruler
" 显示匹配
set showmatch

" 在底部显示按下的指令
set showcmd

" 关闭那些备份缓存文件
set noundofile
set nobackup
set noswapfile


" 设置配色方案
colorscheme evening

" 缩进设置
" 按 >>键或<<键时的缩进宽度
set shiftwidth=4
" 按tab键时的缩进宽度
set tabstop=4
set softtabstop=4
" 禁止将tab缩进转成空格
set noexpandtab

" 设置编码
set encoding=utf-8
set langmenu=zh_CN.UTF-8
set fileencodings=utf-8,gbk,gb18030,gb2312,ucs-bom,cp936,big5,euc-jp,euc-kr,shift-jis,latin1

" 开启语法高亮
syntax on

" 高亮搜索
set hlsearch
" 搜索逐字高亮
set incsearch

" 忽略大小写搜索
set ignorecase
" 智能大小写
set smartcase

" 自动载入
set autoread

" 补全时显示候选项在命令行上方
set wildmenu

" 检测文件类型 
filetype on

" 允许插件
filetype plugin on


"--------------------------------
"       禁止注释行回车自动添加注释
"--------------------------------
autocmd FileType * setlocal formatoptions-=c formatoptions-=r formatoptions-=o

```

> [!info]
> 
> 有一些配置 [Vim](Vim_Note.md) 没有，但 neovim 已经出厂配好了的，所以先查询 neovim 哪些已配过的，以免重复配置。
> 禁止注释行回国自动添加注释，这个设置要放在 `filetype on` 这些文件类型侦测之后，才能生效。

windows 版设置有差异：
```vimscript
set t_Co=256
set termguicolors
```

Windows 下引入配置：
```vimscript
source ~\AppData\Local\nvim\xxx.vim
```

### 特别设置

```vimscript
" 开启透明背景                                                                                                    
func! s:transparent_background()
    hi Normal guibg=NONE ctermbg=NONE
    hi NonText guibg=NONE ctermbg=NONE
endf
autocmd ColorScheme * call s:transparent_background()

```

> [!tip] 
> 
> **注意: 透明背景设置必须放在 colortheme 设置之后!**

#### vim9 新设置

##### 命令行坚排侯选项

默认在 vim9 之前，[命令行模式](Vim_Note.md#命令行模式) 候选项是横排的。

而到了 vim9 版本，可以通过 `set wildoptions=pum`，将候选项坚排显示。

#### python 相关

> [!info] 
> 
> 如果 python 不起作用就使用 `set pythonthreedll` 来指定 [Python](../Python/Python_Note.md) 的 dll

```shell
if has('python3')
	set pyx=3
elseif has('python')
	set pyx=3
endif

" 配置python相关dll路径
set pythonthreedll=I:/Scoop/apps/python-beta/current/python38.dll
```

> [!info] 
>
> neovim 不使用 `set pythonthreedll`
>
>（neovim 使用 `:checkhealth` 检测 python 支持）
>
> neovim 使用 [pip](../Python/Python_Note.md#pip) 安装 `pyvim` 模块

```shell
pip3 install --user --upgrade pynvim
```

---

## vim 集成套件

### SpaceVim

[SpaceVim](https://spacevim.org/)[![SpaceVim Repo](https://img.shields.io/github/stars/SpaceVim/SpaceVim?style=social)](https://github.com/SpaceVim/SpaceVim)

![SpaceVim preview](https://user-images.githubusercontent.com/13142418/228742293-1ca7c173-84a6-461a-9fb5-656d23953e12.png)

### LunarVim

[LunarVim](https://www.lunarvim.org/)[![LunarVim Repo](https://img.shields.io/github/stars/LunarVim/LunarVim?style=social)](https://github.com/LunarVim/LunarVim)

![Lunarvim preview](https://www.lunarvim.org/img/lunarvim_preview.png)

---

## 其他笔记

* [Vim笔记](Vim_Note.md)
* [neovim笔记](Neovim_Note.md)
* [vim及neovim配置](vim及neovim配置.md)
* [vim插件](vim_plugin.md)
* [vim配色笔记](vim_colorscheme_Note.md)

