---
aliases: []
tags:
  - editor
  - neovim
  - nvim
  - config
created: 2023-08-18 19:44:52
modified: 2024-02-22 06:06:10
---

# NeoVim 笔记

---

## 目录

* [neovim 新配置](#nvim_newconfig)

---

## neovim 配置

 这里所说的设置是 「老」的配置，即跟 [Vim](Vim_Note.md) 类似的，使用 `.vim` 文件作为配置文件来配置。

新版本的 neovim 设置，使用 [Lua](../Lua/Lua_Note.md) 方式来进行配置：[neovim 新配置](#neovim%20新配置)。

### neovim 默认设置

[NeoVim 官网](https://neovim.io/) 给出了 neovim 的出厂配置：

```vim

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

> [!note] 
> 
> [出处页面]([Nvim documentation: vim_diff (neovim.io)](https://neovim.io/doc/user/vim_diff.html#nvim-features))

> [!note] 检测 neovim 模块支持状态
> 
>
> 使用 `:checkhealth` 命令，所以检测 neovim 各模块状态。

### 基础配置

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

> [!note] 
> 
> 关于「光标闪烁」：[vim - Neovim：如何设置光标以使光标闪烁？ - Thinbug](https://www.thinbug.com/q/53711349)

---

## <span id="nvim_config_path">配置数据路径</span>

### 配置文件目录

默认当前用户配置文件及数据目录：

Linux（以 Ubuntu20.04 为例）：`~/.config/nvim/`

windows： `C:\Users\用户名\AppData\Local\nvim\`

### 运行时目录

「**运行时目录**」，即 `runtimepath`，[Vim](Vim_Note.md) 或 neovim 除了加载配置文件目录中的 `.vimrc` 或 `init.vim` 或 `.init.lua` 外，还有从运行时目录加载文件。

要查看 runtimepath，可以在 [命令行模式](Vim_Note.md#命令行模式) 下使用 `:echo &runtimepath` 命令。

neovim 大概是按以下目录进行搜寻加载相应文件的：

1. `~/.config/nvim 
2. `/etc/xdg/nvim` 
3. `~/.local/share/nvim/site` 
4. `/usr/local/share/nvim/site` 
5. `/usr/share/nvim/site`

而可见其中 `~/.config/nvim` 是 [配置文件目录](#配置文件目录) 是第一位，然后才是 `~/.local/share/nvim/site` 目录。

使用 [vim-Plug](vim_plugin.md#vim-Plug)，一般会配置让其插件放在 `~/.local/share/nvim/site` 这个目录下（vim-plug，在其配置 `call plug#begin()` 中是可以自定义插件存放路径的，根本 [文档](https://github.com/junegunn/vim-plug?tab=readme-ov-file#example) 说明，其默认放在 `~/.local/share/nvim/plugged`），与 `~/.config/nvim` 这些手动配置文件区分开。

> [!info] 
> 
> `vim.fn.stdpath('config')`：就是 `~/.config/nvim` 这目录。
>
> `vim.fn.stdpath('data')`：就是 `~/.local/share/nvim` 这个目录。
>
>> [!note] 参考资料 
>> 
>> * [如何在Neovim中获取数据目录和配置目录](https://mingda.dev/2022/06/04/getting-config-path-with-lua-in-neovim/)
>> * [Nvim-from-vim - Neovim docs](https://neovim.io/doc/user/nvim.html#nvim-from-vim)
>
>> [!info] 各系统运行时默认目录
>> 
>> * Linux：`~/.local/share/nvim`。
>>   
>> * windows： `./AppData/Local/nvim-data/`

> [!tip] 
> color、autoload 等目录是放在 `~/.local/share/nvim/site`
>
> 而 [Vim](Vim_Note.md) 著名的插件管理工具 [vim-plug](#nvim_plugsin_vimplug) 核心文件 **plug.vim** 默认就是装在 `site/autoload` 
>
> ![image-20200618104543169](./neovim相关.assets/image-20200618104543169.png)

而有些**特殊**的子目录，neovim 会自动加载：

* `colors/`
* `compiler/`
* `ftplugin/`
* `indent/`
* `plugin/`
* `syntax/`

---

## <span id="nvim_newconfig">neovim 新配置</span>

[neovim](https://neovim.io/) 新版本已经使用 [Lua](../Lua/Lua_Note.md) 来构建配置。

使用 `nvim --version` 来查看版本是否是大于 `0.8`，并且有 `LuaJIT` 字样存在，这才符合使用 Lua 来配 neovim。

新配置，可以不使用 `init.vim` 作为配置入口文件，而换成 `init.lua` 为配置入口文件。

* `~/.config/nvim/` 目录是存放所有配置文件，这跟「[老设置](#neovim%20设置)」没什么区别。
 * `~/.config/nvim/init.lua`： 是配置入口文件，主要用来「导入」其他 `.lua` 文件，文件名 `init.lua` 是固定的。
* `~/.config/lua`：这个目录是存放所有配置的目录。

### 配置

`~/.config/nvim` 及 `~/.config/nvim/lua` 俩目录，在新安装 neovim 后，是不存在的，就需要自行新建。

同样的，`~/.config/nvim/init.lua` 这个配置入口文件也得自行新建。

```shell
mkdir ~/.config/nvim
mkdir ~/.config/nvim/lua
touch ~/.config/nvim/init.lua
```

在 `~/.config/nvim/lua` 目录下新建一个 `options.lua` 或 `basic.lua` 文件，文件名可以自定义，用来作为基础配置的配置文件，然后在 `init.lua`「导入」就可以用了。其他配置文件均可以相同方式进行导入加载。

`basic.lua`：  

```lua

-- 显示行号
vim.opt.number = true
-- 显示相对行号
vim.opt.relativenumber = true
-- 高亮当前行
vim.opt.cursorline = true

-- Tab 相关设置
vim.opt.tabstop = 4
vim.opt.softtabstop = 4
vim.opt.shiftwidth = 4
vim.opt.expandtab = false

```

`init.lua` 导入 `basic.lua`，只需通过 `require()` 函数，就能将 `~/.config/nvim/lua` 目录下的那些 `.lua` 文件当成「模块」进行加载：

```lua
require('basic')
```

 `require()` 中只需要 `.lua` 文件的文件名。 如果是 `lua` 目录的目录，也能把目录一并写上。

 如有这么一个配置文件：`~/.config/nvim/lua/a/b1.lua`，要在 `init.lua` 中导入，就可以写成：
 
```lua
require('a/b1') 
-- 或
`require('a.b1')
```

> [!info] 相关资料
>
> * [neovim Wiki](https://github.com/neovim/neovim/wiki)
> * [Quickref - Neovim docs](https://neovim.io/doc/user/quickref.html)
> * [Lua - Neovim docs](https://neovim.io/doc/user/lua.html)
> * [从零开始配置 Neovim(Nvim) - MartinLwx's Blog](https://martinlwx.github.io/zh-cn/config-neovim-from-scratch/)
> * [2022 年的 neovim 配置方案 | Zwlin's Blog](https://blog.zwlin.io/post/2022-nvim/)
> * [**在 neovim 中使用 Lua**](https://github.com/glepnir/nvim-lua-guide-zh)

> [!tip] 
> 
> 如果不想重开 neovim，可以在 [命令行模式](Vim_Note.md#命令行模式) 下输入：`:source %`，就是 `source` 当前文件。

#### vim 设置选项

---

## <span id="nvim_plugins">插件</span>

### <span id="nvim_plugins_vimplug">vim-plug 插件管理器</span>

[junegunn/vim-plug](https://github.com/junegunn/vim-plug) 是在 [Vim](Vim_Note.md) 中非常流行的一款插件管理器。

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

> [!important] 
> neovim 已经转向使用 lua 配置，所以 vim-plug 就有点不太适合现在的 neovim 了。

---

### <span id="nvim_plugins_packernvim">Packer.nvim</span>

[packer.nvim](https://github.com/wbthomason/packer.nvim) 是一款 neovim 的插件管理器。 已停更了！

### <span id="nvim_plugins_lazynvim">Lazy.nvim</span>

[lazy.nvim](https://github.com/folke/lazy.nvim) 同样也是一款 neovim 的插件管理器。

#### 安装 lazy.nvim

1. 向 `init.lua` 中添加以下代码并重启 neovim：

```lua
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git",
    "clone",
    "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable", -- latest stable release
    lazypath,
  })
end
vim.opt.rtp:prepend(lazypath)
```

> [!info] 
> 
> `local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"` ，这句代码，是定义 lazy.nvim 要下载到哪个目录。
> 
> `vim.fn.stdpath("data")`，这是 [运行时目录](#运行时目录)，即 `~/.local/share/nvim` 这个目录。
> 
> `vim.fn.stdpath("data") .. "/lazy/lazy.nvim"` 这一整句就定义了 lazy.nvim 这个插件管理器存放在 `.local/share/nvim/lazy/lazy.nvim` 这个目录。
> 
> 其实 `.local/share/nvim/lazy/` 这个才是 lazy 管理器所定义的插件目录，无论是 lazy.nvim 本身，还是使用 lazy.nvim 管理的其他插件，会放在 `.local/share/nvim/lazy/` 这个目录下。每个插件都是以目录区分，`lazy.nvim` 这个目录便是 lazy.nvim 管理器作为插件本身所在的目录，lazy.nvim 与其他插件为「兄弟」。
> 
> 如下所示，`lazy` 目录下有三个插件：
>
>> ```shell
>> ll .local/share/nvim/lazy/          
>> Permissions Size User       Group      Date Modified    Name
>> drwxr-xr-x     - silascript silascript 2024-02-22 04:41  .
>> drwx------     - silascript silascript 2024-02-22 04:04  ..
>> drwxr-xr-x     - silascript silascript 2024-02-22 04:04  lazy.nvim
>> drwxr-xr-x     - silascript silascript 2024-02-22 04:30  lualine.nvim
>> drwxr-xr-x     - silascript silascript 2024-02-22 04:41  nvim-web-devicons
>> 
>> ```
> 
>
> `vim.opt.rtp:prepend(lazypath)` 这句代码，`rtp`，即 `runtime path`，即 [运行时目录](#运行时目录)。
nvim 进行路径搜索的时候，除已有的路径，还会从 prepend 的路径中查找。如果这句代码没有，那下面的 `require("lazy")` 是找不到的。
>
>> [!note] 参考资料
>> 
>> * [lazy-nvim插件管理器基础入门 - 知乎](https://zhuanlan.zhihu.com/p/638379995?utm_id=0)

2. 重启后 nvim，再向 `init.lua` 添加：`require("lazy").setup(plugins, opts)`，之前那步，只是让 neovim 下载 lazy.nvim 到相应目录，而这个 `require` 才是真正通知 neovim 要加载已经下载的 lazy.nvim 相关的模块。

#### 添加及配置插件

在 `init.lua` 文件中添加：

如果添加什么插件都写在 `init.lua` 文件，那 `init.lua` 文件就太臃肿了，而 lazy.nvim 可以自动「扫描」相关目录，加载相应的插件配置文件，通过这种方式来添加加载使用插件。

大致步骤如下：

1. 在 `.config/nvim/lua` 目录下新建 `plugins` 目录，用来存在插件配置文件的。
2. 在 `init.lua` 文件中添加相应配置，让 lazy.nvim 去扫描这个 `plugins` 目录：

```lua

require("lazy").setup("plugins")

```

或者：

```lua

-- Same as:
require("lazy").setup({{import = "plugins"}})
```

3. 在 `plugins` 目录中添加相应的插件配置文件。
如我要添加 [lualine.nvim](#lualine.nvim) 这个状态栏插件，可以在 `plugins` 目录中新建一个 `lua` 文件，文件名可以自定义，然后添加以下代码：
```lua
return {
    {
        'nvim-lualine/lualine.nvim',
        config = function()
            require('lualine').setup()
        end
    }
}

```

`plugins` 目录中的插件配置文件，可以多个插件共用一个配置文件，这就方便「模块化」添加和配置插件了。

语法很简单，只要把每个插件写在 `return{}` 里，每个插件也使用 `{}` 包起来，如下：

```lua

-- 这个配置文件中有三插件，都是跟界面样式相关的
 return {
	-- nvim-tree
	{
        "nvim-tree/nvim-tree.lua",
        version = "*",
        dependencies = {"nvim-tree/nvim-web-devicons"},
        config = function()
            require("nvim-tree").setup {}
        end
    },


	-- lualine 插件
	{
		"nvim-lualine/lualine.nvim",
		dependencies = { 'nvim-tree/nvim-web-devicons' },
		config = function ()
			require('lualine').setup {
				options = { theme = 'gruvbox' }
			}	
		end,
	},
	
	-- bufferline 插件
	{
		'akinsho/bufferline.nvim', 
		version = "*", 
		dependencies = 'nvim-tree/nvim-web-devicons',
		config = function()
			require("bufferline").setup{}
		end,
	}
}

```

#### lazy.nvim 相关资料

* [从0开始配置基于lua的neovim (lazy.nvim重制版)](https://www.bilibili.com/video/BV1HP411m7mQ)

---

### lualine.nvim

[lualine.nvim](https://github.com/nvim-lualine/lualine.nvim) 是一款状态栏美化插件。

![lualine.nvim screenshot 1](https://user-images.githubusercontent.com/41551030/108650373-bb025580-74bf-11eb-8682-2c09321dd18e.png)

![lualine.nvim screenshot 2](https://user-images.githubusercontent.com/41551030/108650381-bfc70980-74bf-11eb-9245-85c48f0f154a.png)

![lualine.nvim screenshot 3](https://user-images.githubusercontent.com/41551030/108650378-be95dc80-74bf-11eb-9718-82b242ecdd54.png)

### bufferline.nvim

[bufferline.nvim](https://github.com/akinsho/bufferline.nvim) 是一款美化 [Buffer](vim常用操作.md#op_normal_buffer) 的插件。

### nvim-tree

[nvim-tree](https://github.com/nvim-tree/nvim-tree.lua) 是一个显示目录树的插件。

![nvim-tree screenshot](https://user-images.githubusercontent.com/1505378/232662694-8dc494e0-24da-497a-8541-29344293378c.png)

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

---

## 其他笔记

* [Vim笔记](Vim_Note.md)
* [vim及neovim配置](vim及neovim配置.md)
* [vim插件](vim_plugin.md)
* [Vim视频清单](Vim_Videos.md)
* [vim常用操作](vim常用操作.md)
* [vimscript笔记](vimscript_note.md)
