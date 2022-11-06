# NeoVim基本配置
---

## 目录



---

## neovim默认设置

[NeoVim 官网](https://neovim.io/) 给出了 neovim 的出厂配置：
```

Filetype enabled  `:filetype off`
Syntax highlighting enabled `:syntax off` 

autoindent' is enabled
autoread' is enabled
background' defaults to "dark" (unless set automatically by the terminal/UI)
backspace' defaults to "indent,eol,start"
backupdir' defaults to .,~/.local/share/nvim/backup// (|xdg|), auto-created
belloff' defaults to "all"
compatible' is always disabled
complete' excludes "i"
cscopeverbose' is enabled
directory' defaults to ~/.local/share/nvim/swap// (|xdg|), auto-created
display' defaults to "lastline,msgsep"
encoding' is UTF-8 (cf. 'fileencoding' for file-content encoding)
fillchars' defaults (in effect) to "vert:│,fold:·,sep:│"
formatoptions' defaults to "tcqj"
fsync' is disabled
hidden' is enabled
history' defaults to 10000 (the maximum)
hlsearch' is enabled
incsearch' is enabled
joinspaces' is disabled
langnoremap' is enabled
langremap' is disabled
laststatus' defaults to 2 (statusline is always shown)
listchars' defaults to "tab:> ,trail:-,nbsp:+"
nrformats' defaults to "bin,hex"
ruler' is enabled
sessionoptions' includes "unix,slash", excludes "options"
shortmess' includes "F", excludes "S"
showcmd' is enabled
sidescroll' defaults to 1
smarttab' is enabled
startofline' is disabled
switchbuf' defaults to "uselast"
tabpagemax' defaults to 50
tags' defaults to "./tags;,tags"
ttimeoutlen' defaults to 50
ttyfast' is always set
undodir' defaults to ~/.local/share/nvim/undo// (|xdg|), auto-created
viewoptions' includes "unix,slash", excludes "options"
viminfo' includes "!"
wildmenu' is enabled
wildoptions' defaults to "pum,tagfile"

 |man.vim| plugin is enabled, so |:Man| is available by default.
 |matchit| plugin is enabled. To disable it in your config:
 :let loaded_matchit = 1

 |g:vimsyn_embed| defaults to "l" to enable Lua highlighting
```
[出处页面]([Nvim documentation: vim_diff (neovim.io)](https://neovim.io/doc/user/vim_diff.html#nvim-features))




### 检测 neovim 模块支持状态

使用 `:checkhealth` 命令，所以检测 neovim 各模块状态。




---

## 基础配置
因为 neovim 出厂时默认开启了很多原来在 vim 默认没有开启的功能，所以 neovim 的基础配置比 vim 更简洁。

```vim

" 开行号
set number
" 开启相对行号
set relativenumber

" 退格键设置
set backspace=indent,start


" 开启真彩色
if has('termguicolors')
    set termguicolors
endif

" 让光标烁
" 这种简章设置只能让 nvim 在normal 模式中光标闪烁，而且切换为 insert 模式，光标不会变为细线，也就是说默认 neovim 光标两种模式呈现两样式特性失效
" set guicursor=a:blinkon100

" 这种设置能保持 neovim 在 normal 模式和 insert 模式呈现不同的光标样式的特性
" 而两种光标样式都能保持一致的闪烁
set guicursor=n-v-c:block,i-ci-ve:ver25,r-cr:hor20,o:hor50
  \,a:blinkwait700-blinkoff400-blinkon250-Cursor/lCursor
  \,sm:block-blinkwait175-blinkoff150-blinkon175


" 复制高亮
" 这是 neovim 自带功能，无需另加插件实现
au TextYankPost * silent! lua vim.highlight.on_yank {higroup="IncSearch", timeout=1500}

" 如果想在 visual 模式下禁止使用复制高亮，就使用下面这个设置
" au TextYankPost * silent! lua vim.highlight.on_yank {on_visual=false}


" 关闭各种自动生成文件
set noundofile
set nobackup
set noswapfile


" 与底部保持固定间距
set scrolloff=5

" 缩进
set shiftwidth=4
set tabstop=4
set softtabstop=4
" 不要将tab展开成空格
set noexpandtab

" 字符编码
set langmenu=zh_CN.UTF-8
set fileencodings=utf-8,gbk,gb18030,gb2312,ucs-bom,cp936,big5,euc-jp,euc-kr

" 语法高亮及文件类型
filetype plugin on

"--------------------------------
"       禁止注释行回车自动添加注释
"--------------------------------
autocmd FileType * setlocal formatoptions-=c formatoptions-=r formatoptions-=o

```

---

> 关于「光标闪烁」：[vim - Neovim：如何设置光标以使光标闪烁？ - Thinbug](https://www.thinbug.com/q/53711349)

## <span id="nvim_config_path">配置数据路径</span>

关于 neovim 配置及数据目录的路径：

默认当前用户配置文件及数据目录

配置文件：

Linux（以Ubuntu20.04为例）：`~/.config/nvim/`

windows： `C:\Users\用户名\AppData\Local\nvim\`

数据目录：

Linux：`~/.local/share/nvim`

windows： `./AppData/Local/nvim-data/`

color、autoload 等目录是放在 `~/.local/share/nvim/site`

下面介绍的插件管理工具 [vim-plug](#nvim_plugsin_vimplug) 核心文件 **plug.vim** 默认就是装在 `site/autoload` 

![image-20200618104543169](./neovim相关.assets/image-20200618104543169.png)


---

## <span id="nvim_plugins">插件</span>

### <span id="nvim_plugsin_vimplug">vim-plug 插件管理器</span>

[junegunn/vim-plug](https://github.com/junegunn/vim-plug)

Linux 下安装：
```shell
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

windows 下安装：
```shell
iwr -useb https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim |`
    ni "$(@($env:XDG_DATA_HOME, $env:LOCALAPPDATA)[$null -eq $env:XDG_DATA_HOME])/nvim-data/site/autoload/plug.vim" -Force
```

更详细内容：[vim plugin](#vimplugin_plug)

---

### <span id="nvim_plugins_basic">基础插件</span>

基础插件，大概围绕什么注释、光标样式等编辑基础的功能增加。

* [preservim/nerdcommenter](https://github.com/preservim/nerdcommenter)
* [tpope/vim-surround](https://github.com/tpope/vim-surround)
* [vim-cursorword](https://github.com/itchyny/vim-cursorword)
* ~~[machakann/vim-highlightedyank](https://github.com/machakann/vim-highlightedyank)~~
* [jszakmeister/vim-togglecursor](https://github.com/jszakmeister/vim-togglecursor)
* [auto-pairs](https://github.com/jiangmiao/auto-pairs)
* [wellle/targets.vim](https://github.com/wellle/targets.vim)
* [terryma/vim-expand-region](https://github.com/terryma/vim-expand-region)


