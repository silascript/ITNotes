---
aliases: []
tags:
  - editor
  - neovim
  - nvim
  - config
  - plugin
created: 2023-08-18 19:44:52
modified: 2024-04-08 01:14:10
---

# NeoVim 笔记

---

## 目录

* [neovim 新配置](#nvim_newconfig)
* [插件](#nvim_colourscheme)
	* [lazynvim](#nvim_plugins_lazynvim)
	* [编辑支持](#nvim_plugins_editesupport)
	* [状态栏及Buffer](#nvim_plugins_sbline)
	* [目录树](#nvim_plugins_dirtree)
	* [缩进](#nvim_plugins_indent)
	* [snippet](#nvim_plugins_snippets)
	* [补全](#nvim_plugins_completion)
* [配色](#nvim_colourscheme)

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
* `~/.config/nvim/lua`：这个目录是存放所有配置的目录。

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
-- 状态栏样式
-- vim.opt.laststatus = 2
-- 命令行高

-- 开启真彩
vim.opt.termguicolors = true

-- 与底部保持固定间距
vim.opt.scrolloff = 5

-- Tab 相关设置
vim.opt.tabstop = 4
vim.opt.softtabstop = 4
vim.opt.shiftwidth = 4
vim.opt.expandtab = false

-- 编码
vim.opt.langmenu = "zh_CN.UTF-8"
vim.opt.fileencodings = "utf-8,gb18030,gb2312,ucs-bom,cp936,big5,euc-jp,euc-kr"

-- 搜索
vim.opt.incsearch = true
vim.opt.hlsearch = true
vim.opt.ignorecase = true
vim.opt.smartcase = true

vim.opt.completeopt = "menu,menuone,noinsert"

-- 补全显示的行数
vim.opt.pumheight = 10

-- 取消注释行回车自动注释
vim.api.nvim_create_autocmd(
    {"FileType"},
    {
        command = "setlocal formatoptions-=c formatoptions-=r formatoptions-=o"
    }
)

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
require('a.b1')
```

> [!info] 相关资料
>
> * [neovim Wiki](https://github.com/neovim/neovim/wiki)
> * [Quickref - Neovim docs](https://neovim.io/doc/user/quickref.html)
> * [Lua - Neovim docs](https://neovim.io/doc/user/lua.html)
> * [从零开始配置 Neovim(Nvim) - MartinLwx's Blog](https://martinlwx.github.io/zh-cn/config-neovim-from-scratch/)
> * [2022 年的 neovim 配置方案 | Zwlin's Blog](https://blog.zwlin.io/post/2022-nvim/)
> * [**在 neovim 中使用 Lua**](https://github.com/glepnir/nvim-lua-guide-zh)
> * [Neovim：插件入门 - 知乎](https://zhuanlan.zhihu.com/p/596932328)

> [!tip] 
> 
> 如果不想重开 neovim，可以在 [命令行模式](Vim_Note.md#命令行模式) 下输入：`:source %`，就是 `source` 当前文件。

#### 快捷键设置

##### 相关资料

* [Neovim 的快捷键配置](https://www.zhihu.com/tardis/zm/art/435680085?source_id=1005)

### NVIM_APPNAME

[nvim](https://neovim.io/) 0.9 版本加了一个东西：`NVIM_APPNAME`。

这东西能使用 nvim 能实现不同配置的隔离。

在 `.bashrc` 或 `.zshrc` 中添加以下类似的别名（`alias`）代码，就能让 nvim 拥有不同的配置版本。

```sh
# nvim 不同的配置
alias tiny-nvim="NVIM_APPNAME=tiny-nvim nvim"
```

> [!info] 
> 
> `alias tiny-nvim` 这是声明一个 nvim 的别名，它不是简单的 `nvim` 的软连接更名，而是连配置文件都指向不同目录。
> 
> `NVIM_APPNAME=tiny-nvim` 是指定 `.local/share/` 目录下子目录为 `tiny-nvim`，即 nvim 的 [运行时目录](#运行时目录)，这在 `source` 完 `xxxrc`，别名生效后，第一次调用这个别名启动 nvim 时，系统自动为其生成对应的 [运行时目录](#运行时目录)，这示例中就会生成 `~/.local/share/tiny-nvim` 目录。
> 
> 而不同配置，也还得有不同的配置目录。这个目录就没有自动生成了，得自己手动创建。位置在 `~/.config/` 目录下，与 `nvim` 那个目录是同级。即这个配置的 nvim 与默认的 nvim 是兄弟关系。

---

## <span id="nvim_plugins">插件</span>

### 插件集合

网上有好心的网友收集整理 neovim 插件集合：

* [awesome-neovim](https://github.com/rockerBOO/awesome-neovim)

### <span id="nvim_plugins_vimplug">vim-plug</span>

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
> 
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

> [!info] 
>
> 可以 `import` 多个目录，甚至是子目录，因为 lazy 不会「遍历」子目录，如果有子目录，得再多加一个 `import` 语句：
>  
> ```lua
> require("lazy").setup(
> {
> 	{ import = "plugins" },
>	{ import = "plugins/ui" },
> })
>
> ```
> 

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

#### lazy 语法

##### 插件配置

* `dependencies`：插件启动依赖配置
* `enabled`：开启或禁用某插件，如：`enable = false` 这就是禁用。

###### config

```lua
config = function()
	-- 配置语句写这里
end
```

###### event

`event`，指定该插件什么事件下触发，如： `event = "VeryLazy"`。

如果有多个值，可以使用大括号 `{}` 括起来，如：`event = {"BufReadPre","BufNewFile"}`。

这些值实际是 [vim events](Vim_Note.md#events) 和 neovim 本来就有的，可以通过命令：`:help {events}`

其他配置项参考：[lazy.nvim README-plugin-spec](https://github.com/folke/lazy.nvim?tab=readme-ov-file#-plugin-spec)

#### lazy.nvim 相关资料

* [从0开始配置基于lua的neovim (lazy.nvim重制版)](https://www.bilibili.com/video/BV1HP411m7mQ)

---

### <span id="nvim_plugins_starup">开始页面</span>

#### dashboard-nvim

[dashboard-nvim](https://github.com/nvimdev/dashboard-nvim) 是 [GitHub](../Git/Git_Note.md#git_github) 上「Starred」非常高的开始页面插件。

![dashboard-nvim screenshot](https://user-images.githubusercontent.com/41671631/215015845-b13343c4-427e-45d6-9f92-267ab909eff1.png)

dashboard 分 `Hyper` 和 `Doom` 两种样式。

`Hyper` 可以对 `shortcut` 配置。

```lua
shortcut = {                                    
	{ desc = '󰊳 Lazy', group = '@property', action = 'Lazy', key = 'l' },
	-- { desc = '󰊳 Update', group = '@property', action = 'Lazy update', key = 'u' },
}
```

`Doom` 则是对 `center` 进行配置。

```lua
center = {
	{ action = "ene | startinsert", desc = " New file", icon = " ", key = "n" },
	{ action = "LazyExtras", desc = " Lazy Extras", icon = " ", key = "x" },
	{ action = "Lazy", desc = " Lazy", icon = "󰒲 ", key = "l" },
	{ action = "qa", desc = " Quit", icon = " ", key = "q" },
```

#### dashboard 相关资料

* [从零开始配置 vim(16)——启动界面配置 - 知乎](https://zhuanlan.zhihu.com/p/555057672?utm_id=0)

#### alpha-nvim

[alpha-nvim](https://github.com/goolord/alpha-nvim) 这个是跟 [vim-startify](vim_plugin.md#vim-startify) 很像的插件。

![alpha-nvim screenshot](https://user-images.githubusercontent.com/24906808/133367667-0f73e9e1-ea75-46d1-8e1b-ff0ecfeafeb1.png)

这插件预设了几个不同风格的配置，在 `setup` 中 `require` 下就马上能用了。

`startify` 风格是最简单的，只依赖了一个图标插件：

```lua
{
	'goolord/alpha-nvim',
	dependencies = { 'nvim-tree/nvim-web-devicons' },
	config = function ()
		require'alpha'.setup(require'alpha.themes.startify'.config)
	end
};
```

配置一定的样式：

```lua
-- alpha-nvim
{
	"goolord/alpha-nvim",
	dependencies = {"nvim-tree/nvim-web-devicons"},
	enabled = true,
	-- enabled = false,
	event = {"VimEnter"},
	config = function()
		local alpha = require "alpha"

		-- 不同的样式：dashboard startify
		-- 对样式什么都不配置，可以直接将样式加入到setup中
		-- require "alpha".setup(require "alpha.themes.startify".config)
		-- require "alpha".setup(require'alpha.themes.dashboard'.config)

		-- 对样式进行设置
		-- local dashboard = require "alpha.themes.dashboard"
		local startify = require "alpha.themes.startify"
		-- 设置 header
		-- 使用到了figlet
		startify.section.header.val = vim.split(vim.fn.system("figlet -f 'ANSI Shadow' 'HELLO NVIM' "), "\n")
		-- dashboard.section.header.val = {"NEOVIM"}

		-- 加载样式配置
		-- alpha.setup(dashboard.config)
		alpha.setup(startify.config)
	end
}
```

可以对 `header`、`footer` 等部分进行单独设置：

```lua
{
	"goolord/alpha-nvim",
	dependencies = {"nvim-tree/nvim-web-devicons"},
	enabled = true,
	-- enabled = false,
	-- lazy = true,
	event = {"VimEnter"},
	config = function()
		local alpha = require "alpha"

		-- 不同的样式：dashboard startify
		-- 对样式什么都不配置，可以直接将样式加入到setup中
		-- require "alpha".setup(require "alpha.themes.startify".config)
		-- require "alpha".setup(require'alpha.themes.dashboard'.config)

		-- 对样式进行设置
		-- local dashboard = require "alpha.themes.dashboard"
		local startify = require "alpha.themes.startify"

		-- 设置 header
		-- header.val 接受的就是一个字典，以回车换行分隔的字符字典
		-- 所以将 figlet生成的 ASII字符以回车换行符切割
		-- startify.section.header.val = vim.split(vim.fn.system("figlet -f 'ANSI Shadow' 'HELLO NVIM' "), "\n")
		local header_str = vim.split(vim.fn.system("figlet -f 'ANSI Shadow' 'HELLO NVIM' "), "\n")
		startify.section.header.val = header_str
		startify.section.header.opts.position = "center"

		-- mru 设置
		-- 把mru 或 mru_cwd 禁用
		startify.section.mru_cwd.val = {{type = "padding", val = 0}}
		-- startify.section.mru.val = {{type = "padding", val = 0}}

		-- 加载样式配置
		-- alpha.setup(dashboard.config)
		alpha.setup(startify.config)

		-- footer 设置
		-- 使用回调函数
		-- 因为需要获取插件相关信息
		vim.api.nvim_create_autocmd(
			"User",
			{
				callback = function()
					local stats = require("lazy").stats()
					local plugins_count = stats.loaded .. "/" .. stats.count
					-- 获取启动时长
					local stms = math.floor(stats.startuptime * 100) / 100
					local fline1 = " " .. plugins_count .. " plugins loaded in " .. stms .. "ms"

					startify.section.footer.val = {
						{
							type = "text",
							-- val = "footer",
							val = fline1,
							opts = {
								position = "center"
								hl = "Number"
							}
						}
					}
					pcall(vim.cmd.AlphaRedraw)
				end
			}
		)

		end
	}
}

```

##### alpha 相关资料

* [alpha-nvim-doc](https://github.com/goolord/alpha-nvim/blob/main/doc/alpha.txt)

#### startup.nvim

[starup.nvim](https://github.com/startup-nvim/startup.nvim) 是一个完全自由定制的开始页面插件。

#### veil

[veil](https://github.com/willothy/veil.nvim)

<video src="https://private-user-images.githubusercontent.com/38540736/228553706-b68e99a7-c4d6-4803-a06e-4e3bb12109ea.mp4"></video>

### <span id="nvim_plugins_sbline">状态栏及 Buffer</span>

#### lualine.nvim

[lualine.nvim](https://github.com/nvim-lualine/lualine.nvim) 是一款状态栏美化插件。

![lualine.nvim screenshot 1](https://user-images.githubusercontent.com/41551030/108650373-bb025580-74bf-11eb-8682-2c09321dd18e.png)

![lualine.nvim screenshot 2](https://user-images.githubusercontent.com/41551030/108650381-bfc70980-74bf-11eb-9245-85c48f0f154a.png)

![lualine.nvim screenshot 3](https://user-images.githubusercontent.com/41551030/108650378-be95dc80-74bf-11eb-9718-82b242ecdd54.png)

```lua
{
	"nvim-lualine/lualine.nvim",
	dependencies = {"nvim-tree/nvim-web-devicons"},
	-- event = "InsertEnter,",
	-- event = "VimEnter",
	event = "BufEnter",
	-- event = "BufWinEnter",
	-- event = {"VimEnter","BufReadPre","BufNewFile"},
	config = function()
		require("lualine").setup {
			options = {
				-- 指定样式
				--theme = "gruvbox"
				--theme = "ayu_mirage"
				--theme = "everforest"
				--theme = "OceanicNext",
				theme = "material"
				-- 指定分隔符样式，默认认是尖角的
				-- section_separators = { left = '', right = '' },
				-- component_separators = { left = '', right = '' }
				-- 这是默认的分隔符样式，不用专门指定
				-- component_separators = { left = '', right = ''},
				-- section_separators = { left = '', right = ''},
				-- 如果想要像theme列表中显示那样平的，就把两个分隔符置空
				-- 都置空就不用分左右
				-- section_separators = '', component_separators = ''
			}
		}
	end
},

```

样式列表：[lualine.nvim/THEMES.md](https://github.com/nvim-lualine/lualine.nvim/blob/master/THEMES.md)

当前 `theme` 设置项还能设置为 `auto`，这时，状态栏的配色就使用整个 nvim 全局 [配色](#配色)。

```lua
{
	"nvim-lualine/lualine.nvim",
	dependencies = {"nvim-tree/nvim-web-devicons"},
	event = "BufEnter",
	config = function()
		require("lualine").setup {
			-- 设置样式
			options = {
				-- 使用 auto 意味着状态栏配色使用全局配色
				-- 即使用 vim.cmd.colorscheme 指定的配色
				theme = "auto"
			}
		}
	end
}
```

> [!info] 相关资料
> 
> * [状态栏 | LunarVim](https://www.lunarvim.org/zh-Hans/docs/master/configuration/appearance/statusline)

#### lsp-progress

[linrongbin16/lsp-progress.nvim](https://github.com/linrongbin16/lsp-progress.nvim) 这是一个在状态栏上显示 [LSP](Vim_LSP_Complete.md) 信息的插件，算是状态栏插件的补充的插件。 

这插件与 [lualine.nvim](#lualine.nvim) 搭配使用，效果极佳！

##### 安装和配置

简单安装：

```lua
 "linrongbin16/lsp-progress.nvim",
lazy = true,
event = {"BufReadPost", "BufAdd", "BufNewFile"},
config = function()
	require("lsp-progress").setup()
end
```

在 [lualine.nvim](#lualine.nvim) 中添加组件，不过不要直接加，得用个函数包一下。

一般加在 lualine 的「c 位」，就是文件名那个位置：

```lua
require("lualine").setup({
  sections = {
    lualine_c = {
      -- invoke `progress` here.
      function()
        return require('lsp-progress').progress()
      end,
    },
    ...
  }
})

```

再在 lualine 中加个「监听」，随时刷新 LSP 信息：

```lua
-- listen lsp-progress event and refresh lualine
vim.api.nvim_create_augroup("lualine_augroup", { clear = true })
vim.api.nvim_create_autocmd("User", {
  group = "lualine_augroup",
  pattern = "LspProgressStatusUpdated",
  callback = require("lualine").refresh,
})
```

这样配后，此组件已经可以用了。不过只是在状态栏显示「LSP」字样，并不能显示更多的信息。如果想显示诸如 LSP 名称什么的，得进一样配置：

```lua
-- lsp-progress.nvim
{
	"linrongbin16/lsp-progress.nvim",
	lazy = true,
	event = {"BufReadPost", "BufAdd", "BufNewFile"},
	config = function()
		require("lsp-progress").setup(
			{
				client_format = function(client_name, spinner, series_messages)
					if #series_messages == 0 then
						return nil
					end
					return {
						name = client_name,
						body = spinner .. " " .. table.concat(series_messages, ", ")
					}
				end,
				format = function(client_messages)
					--- @param name string
					--- @param msg string?
					--- @return string
					local function stringify(name, msg)
						return msg and string.format("%s %s", name, msg) or name
					end

					local sign = "" -- nf-fa-gear \uf013
					local lsp_clients = vim.lsp.get_active_clients()
					local messages_map = {}
					for _, climsg in ipairs(client_messages) do
						messages_map[climsg.name] = climsg.body
					end

					if #lsp_clients > 0 then
						table.sort(
							lsp_clients,
							function(a, b)
								return a.name < b.name
							end
						)
						local builder = {}
						for _, cli in ipairs(lsp_clients) do
							if type(cli) == "table" and type(cli.name) == "string" and string.len(cli.name) > 0 then
								if messages_map[cli.name] then
									table.insert(builder, stringify(cli.name, messages_map[cli.name]))
								else
									table.insert(builder, stringify(cli.name))
								end
							end
						end
						if #builder > 0 then
							return sign .. " " .. table.concat(builder, ", ")
						end
					end
					return ""
				end
			}
		)
	end
}
```

以上配置就是只在状态栏显示 LSP 的名称。

其他需求配置可以参考：[Advanced Configuration · linrongbin16/lsp-progress.nvim Wiki · GitHub](https://github.com/linrongbin16/lsp-progress.nvim/wiki/Advanced-Configuration)，常用配置可以直接拷过来用，非常方便。

##### 类似插件

* [arkav/lualine-lsp-progress](https://github.com/arkav/lualine-lsp-progress)

#### linefly

[nvim-linefly](https://github.com/bluz71/nvim-linefly) 是一个非常小巧的 statusline 插件。它使用 nvim 的 colorscheme 的 [配色](#配色)。

```lua
{
	'bluz71/nvim-linefly',
	-- enabled = false,
	enabled = true,
	config =function ()
		vim.g.linefly_options = {
			with_lsp_status = false,
		}
	end
}
```

#### hradline

[nvim-hardline](https://github.com/ojroques/nvim-hardline) 同样是一个简洁轻量级的 statusline 插件。

它内置了一些 theme 可用。

![hardline screenshot](https://user-images.githubusercontent.com/23409060/188603562-aff6f003-69bc-4bd2-b4c5-83007f338d25.png)

```lua
{
	"ojroques/nvim-hardline",
	enabled = true,
	config = function()
		require("hardline").setup(
			{
				-- theme = "catppuccin_minimal"
				-- theme = "codeschool_dark"
				-- theme = "jellybeans"
				theme = "nord"
				-- theme = "nordic"
				-- theme = "one"
				-- theme = "gruvbox_minimal"
				-- theme = "gruvbox"
			}
		)
	end
}
```

#### bufferline.nvim

[bufferline.nvim](https://github.com/akinsho/bufferline.nvim) 是一款美化 [Buffer](vim常用操作.md#op_normal_buffer) 的插件。

---

### <span id="nvim_plugins_motion">移动</span>

#### hop.nvim

[hop.nvim](https://github.com/smoka7/hop.nvim) 类似 [easymotion](vim_plugin.md#easymotion) 的移动跳转插件。

##### 命令

###### 行级

* `:HopLine`：可视范围**行**级跳转。
* `:HopLineAC`：当前光标所在位置**往下**可视范围**行**级跳转。
* `:HopLineBC`：当前光标所在位置**往上**可视范围**行**级跳转。

###### 单词级跳转

* `:HopWord`：可视范围**单词**跳转。
* `:HopWordAC`：当前光标所在位置**往下**可视范围**单词**级跳转。
* `:HopWordBC`：当前光标所在位置**往上**可视范围**单词**级跳转。
* `:HopWordCurrentLine`：光标所在行，**行内**单词跳转。
* `:HopWordCurrentLineAC`：当前行内**往后**（**往右**）单词级跳转。
* `:HopWordCurrentLineBC`：当前行内**往前**（**往左**）单词级跳转。

###### 字符级

这类跳转，就如名称所言「Anywhere」，可以跳转到任何地方，其实就是**字符**级跳转。功能很很强，但不太实用。因为跳转标记符会把目标位置字符都盖掉了，对下一步跳转操作造成麻烦。

* `:HopAnywhere`：可见范围所有**字符**跳转。=
* `:HopAnywhereAC`：当前光标**向下**可视范围所有**字符**跳转。
* `:HopAnywhereBC`：当前光标**向上**可视范围所有**字符**跳转。

> [!tip] 命令小结
>  
> * `Hop*AC`：往后/往右/往下，`A` 其实就是 `after`
> * `Hop*BC`：往前/往左/往上，`B` 其实就是 `before`，这与 [vim操作](vim常用操作.md) 中的 `B` 保持一致。
> * `Hop*MW`：命令中有 `MW`，是 `multi-window` 的意思，多 window 模式。

自用配置：

```lua
{
	"smoka7/hop.nvim",
	event = {"BufRead"},
	config = function()
		require("hop").setup(
			{
				-- 行间跳转
				-- 向下行间跳转
				vim.api.nvim_set_keymap("n", "<leader><leader>j", "<cmd>HopLineAC<cr>", {silent = true}),
				-- 向上行间跳转
				vim.api.nvim_set_keymap("n", "<leader><leader>k", "<cmd>HopLineBC<cr>", {silent = true}),
				-- 单词级跳转
				-- 可视范围所有地方单词跳转
				vim.api.nvim_set_keymap("n", "<leader><leader>w", "<cmd>HopWord<cr>", {silent = true}),
				-- 当前行往下可视范围所有单词跳转
				vim.api.nvim_set_keymap("n", "<leader><leader>wj", "<cmd>HopWordAC<cr>", {silent = true}),
				-- 当前行往上可视范围所有单词跳转
				vim.api.nvim_set_keymap("n", "<leader><leader>wk", "<cmd>HopWordBC<cr>", {silent = true}),
				-- 当前行所有单词跳转
				vim.api.nvim_set_keymap("n", "<leader><leader>wl", "<cmd>HopWordCurrentLine<cr>", {silent = true}),
				-- 当前行向左所有单词跳转
				vim.api.nvim_set_keymap("n", "<leader><leader>h", "<cmd>HopWordCurrentLineBC<cr>", {silent = true}),
				-- 当前行向右所有单词跳转
				vim.api.nvim_set_keymap("n", "<leader><leader>l", "<cmd>HopWordCurrentLineAC<cr>", {silent = true})
			}
		)
	end
}
```

---

### modicator

[modicator](https://github.com/mawkler/modicator.nvim) 是一个动态改变行号颜色的插件。

![modicator screenshot](https://user-images.githubusercontent.com/15816726/215295831-299dc732-85ae-4668-9e7b-e88cd499f18a.gif)

---

### <span idj="nvim_plugins_themeswitch">配色切换</span>

#### themery

[themery.nvim](https://github.com/zaldih/themery.nvim) 是一个 [配色](#配色) 切换的插件。

![themery screenshot](https://github.com/zaldih/themery.nvim/raw/main/docs/header.png)

#### colorbox

[colorbox.nvim](https://github.com/linrongbin16/colorbox.nvim)

---

### <span id="nvim_plugins_dirtree">目录树</span>

#### nvim-tree

[nvim-tree](https://github.com/nvim-tree/nvim-tree.lua) 是一个显示目录树的插件。

![nvim-tree screenshot](https://user-images.githubusercontent.com/1505378/232662694-8dc494e0-24da-497a-8541-29344293378c.png)

---

### <span id="nvim_plugins_indent">缩进</span>

#### indent-blankline

[indent-blankline.nvim](https://github.com/lukas-reineke/indent-blankline.nvim) 是一个显示缩进线的插件。

「彩虹」缩进线配置：

```lua
{
	"lukas-reineke/indent-blankline.nvim",
	main = "ibl",
	-- enabled = false,
	event = "BufReadPost",
	config = function()
		local highlight = {}

		local hooks = require "ibl.hooks"
		-- create the highlight groups in the highlight setup hook, so they are reset
		-- every time the colorscheme changes
		hooks.register(
			hooks.type.HIGHLIGHT_SETUP,
			function()
				vim.api.nvim_set_hl(0, "RainbowRed", {fg = "#E06C75"})
				vim.api.nvim_set_hl(0, "RainbowYellow", {fg = "#E5C07B"})
				vim.api.nvim_set_hl(0, "RainbowBlue", {fg = "#61AFEF"})
				vim.api.nvim_set_hl(0, "RainbowOrange", {fg = "#D19A66"})
				vim.api.nvim_set_hl(0, "RainbowGreen", {fg = "#98C379"})
				vim.api.nvim_set_hl(0, "RainbowViolet", {fg = "#C678DD"})
				vim.api.nvim_set_hl(0, "RainbowCyan", {fg = "#56B6C2"})
			end
		)

		require("ibl").setup(
			{
				indent = {highlight = highlight}
			}
		)
	end
	-- opts = {}
}
```

#### hlchunck

[hlchunk.nvim](https://github.com/shellRaining/hlchunk.nvim) 同样也是一个显示缩进线的插件。它比 [indent-blankline](#indent-blankline) 更轻量。

![hlchunk screenshot 1](https://raw.githubusercontent.com/shellRaining/img/main/2302/23_hlchunk2.png)

![hlchunk screenshot 2](https://raw.githubusercontent.com/shellRaining/img/main/2303/08_hlchunk8.gif)

简单配置就很好用了：

```lua
{
	"shellRaining/hlchunk.nvim",
	-- enabled = false,
	event = {"UIEnter"},
	config = function()
		require("hlchunk").setup({
			blank = {
				enable = false,
			}
		})
	end
}
```

---

### nvim-treesitter

[nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter) 高亮增强插件。

基础配置：

```lua
{
	'nvim-treesitter/nvim-treesitter',
	config = function()
		require('nvim-treesitter.configs').setup({

				ensure_installed = { "c","cpp","html", "css", "json","vim", "lua", "go","javascript", "typescript", "tsx"},
				highlight = { 
					enable = true,
					additional_vim_regex_highlighting = false
				},
				-- indent = { enable = true },
				-- 不同括号颜色区分
				rainbow = {
					enable = true,
					extended_mode = true,
					max_file_lines = nil,
				},

		})
		 
	end,
}


```

#### nvim-treesitter 命令

nvim-treesitter 的命令都是以 `TS` 开头的。

要装哪个语言的就用 `TSInstall 语言名称`，比如 `:TSInstall javascript`。

[nvim-treesitter 支持高亮的语言列表](https://github.com/nvim-treesitter/nvim-treesitter?tab=readme-ov-file#supported-languages)

其他命令

* `:TSInstallInfo`： 可以查看语言安装情况。
* `:TSBufToggle highlight`：开启或关闭高亮

#### 相关资料

* [从零开始配置vim(21)——lsp简介与treesitter 配置-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2127555)
* [Neovim 代码高亮插件 nvim-treesitter 的安装与配置](https://www.zhihu.com/tardis/bd/art/441818052)
* [Neovim 代码高亮插件 nvim-treesitter 的安装与配置](https://www.zhihu.com/tardis/bd/art/441818052?source_id=1001)

### <span id="nvim_plugins_editesupport">编辑支持</span>

#### nvim-autopairs

[nvim-autopairs](https://github.com/windwp/nvim-autopairs) 是个成对符号补全插件。

#### nvim-ts-autotag

[nvim-ts-autotag](https://github.com/windwp/nvim-ts-autotag) 是一个自动关闭标签的插件。

对于像 [Html](../Frontend/Html_Note.md)、[XML](../XML/xml_dtd.md) 等标签类型的文件而言，标签输入时自动关闭是必备功能，但对于 [nvim-autopairs](#nvim-autopairs) 只能补全标点符号这种成对补全插件而言，就有点力不从心了。而 nvim-ts-autotag 这个插件就是实现了自动关闭标签功能而存在的。

简单配置就能用：

```lua
{
	"windwp/nvim-ts-autotag",
	dependencies = {"nvim-treesitter/nvim-treesitter"},
	event = {"BufReadPost"},
	config = function()
		require("nvim-ts-autotag").setup()
	end
}
```

#### nvim-cursorline

[nvim-cursorline](https://github.com/yamatsum/nvim-cursorline) 这插件是让光标所在的单词高亮的小工具。

这插件除了主要功能高亮当前单词，还附赠了「动态」高亮当前行（原生配置只开启当前行高亮是固定的，一直高亮，而这个插件是让这高亮「动」起来），其实没有什么用。

```lua
{
	"yamatsum/nvim-cursorline",
	config = function()
		require('nvim-cursorline').setup({
			cursorword = {
				enable = true,
				-- 最少多个字符的单词触发高亮
				min_length = 3,
				-- 高亮形式
				hl = { underline = true },
			  },
			-- 禁用动态高亮当前行功能
			cursorline = {
				enable = false,
			}
		})
	end,
}
```

#### yanky

[yanky.nvim](https://github.com/gbprod/yanky.nvim) 是一个复制增强插件。

我装这插件主要是为了复制高亮这个功能，但实际上这个插件的功能不止复制高亮这点。

```lua
{
	"gbprod/yanky.nvim",
	dependencies = {"kkharji/sqlite.lua"},
	lazy = true,
	event = {"BufReadPost", "BufAdd", "BufNewFile"},
	config = function()
		require("yanky").setup(
			{
				system_clipboard = {
					sync_with_ring = false
				},
				highlight = {
					on_put = true,
					on_yank = true,
					timer = 2500
				}
			}
		)
	end
}
```

> [!info] 
> 
> `highlight` 中 `timer` 是高亮时值。

#### nvim-surround

[nvim-surround](https://github.com/kylechui/nvim-surround) 是大名鼎鼎的 [Surround](vim_plugin.md#Surround) 的 neovim 版本。

<video src="https://user-images.githubusercontent.com/48545987/178679494-c7d58bdd-d8ca-4802-a01c-a9444b8b882f.mp4" type="video/mp4"></video>

### <span id="nvim_plugins_comments">注释</span>

#### Comment.nvim

[Comment.nvim](https://github.com/numToStr/Comment.nvim) 是 nvim 上的一款注释插件。

默认注释和取消注释的快捷键为：`gcc`，而使用 [Visual 模式](vim常用操作.md#op_visual) 时，默认使用 `gc` 来注释及取消注释。

在配置快捷键时，同样能使用 `<leader>` 键，而默认仍为 `\`。

这个插件定义快捷键太细腻了，这是优势也是劣势，得记过多的快捷键，其实有一些操作是可以共用快捷键，如日常最常用的：**单行注释**及使用 [Visual 模式](vim常用操作.md#op_visual)[Visual 模式](vim常用操作.md#op_visual) 选取**多行注释**，这两种操作完全可以共用同一个快捷键。

```lua
{
	'numToStr/Comment.nvim',
	config = function()
		require('Comment').setup({

			-- NORMAL模式
			toggler = {
				line = '<leader>cc',
			 },
			
			-- VISUAL模式
			opleader = {
				line = '<leader>cc',
			},

		})
	end,
},

```

#### nvim-ts-context-commentstring

[nvim-ts-context-commentstring](https://github.com/JoosepAlviste/nvim-ts-context-commentstring) 是一个注释增强插件。

它使用 [nvim-treesitter](#nvim-treesitter)，在 [Comment.nvim](#Comment.nvim) 插件基础上，优化了代码注释效果。

比如在 [Html](../Frontend/Html_Note.md) 文件中的 [javascript](../JS/JS_Note.md) 代码，如果使用 [Comment.nvim](#Comment.nvim) 插件，只能是 `<!-- -->` 这种 html 风格的注释。而加上了 ts-context-commentstring 这个插件后，则会变为 `//` 这种 javascript 风格的注释。可以说这个插件是对 [Comment.nvim](#Comment.nvim) 插件非常好的补充。

![ts-context-commentstring](https://user-images.githubusercontent.com/9450943/185669275-cdfa7fa4-092e-439b-822e-330559a7d4d7.gif)

##### 配置

要使用这个插件，有两个**前置条件**：

1. 安装了 [nvim-tree](#nvim-tree) 插件
2. 安装了 [Comment.nvim](#Comment.nvim) 等注释插件。事实上 ts-context-commentstring 这插件不止针对于 [Comment.nvim](#Comment.nvim) 这插件作出适配，它也支持其他注释插件，并给出了「适配」方案，具体参考：[ts-context-commentstring pre-comment-hook](https://github.com/JoosepAlviste/nvim-ts-context-commentstring/wiki/Integrations#plugins-with-a-pre-comment-hook)。

> [!info] 支持注释插件列表
> 
> * [b3nj5m1n/kommentary](https://github.com/JoosepAlviste/nvim-ts-context-commentstring/wiki/Integrations#kommentary)
> * [terrortylor/nvim-comment](https://github.com/JoosepAlviste/nvim-ts-context-commentstring/wiki/Integrations#nvim-comment)
> * [numToStr/Comment.nvim](https://github.com/JoosepAlviste/nvim-ts-context-commentstring/wiki/Integrations#commentnvim)
> * [echasnovski/mini.nvim/mini-comment](https://github.com/JoosepAlviste/nvim-ts-context-commentstring/wiki/Integrations#minicomment)
> * [tpope/vim-commentary](https://github.com/JoosepAlviste/nvim-ts-context-commentstring/wiki/Integrations#vim-commentary)

而要想让这个插件生效得配两个地方：

1. ts-context-commentstring 本身的配置

```lua
{
	"JoosepAlviste/nvim-ts-context-commentstring",
	dependencies = {"nvim-treesitter/nvim-treesitter"},
	-- lazy = true,
	event = {"BufReadPost"},
	config = function()
		require("ts_context_commentstring").setup {}
	end
}

```

2. 在 [Comment.nvim](#Comment) 配置中加上以下代码：

```lua
pre_hook = require("ts_context_commentstring.integrations.comment_nvim").create_pre_hook(),
```

#### comment-box

[comment-box](https://github.com/LudoPinelli/comment-box.nvim) 是一个加各样式注释的插件。

![comment-box screenshot 1](https://raw.githubusercontent.com/LudoPinelli/comment-box.nvim/main/imgs/cb-quickstart.png)

##### comment-box 命令

![comment-box commands screenshot](https://raw.githubusercontent.com/LudoPinelli/comment-box.nvim/main/imgs/cb-nomenclature.png)

命令都是以 `CB` 开头。包括 `CB` 这个命令起始部分，共有 5 个部分，最后一个「可选外」，其余都是「必选」。

2. 一个字符，指的是那个「框框」（就是 comment-box 那个「box」）放置在页面的位置：`c` 是 center 中间；`l` 是 left 左边；`r` 是 right 右边。
3. 一个字符代表的是文字的位置，同样有 `c`、`l` 和 `r`，另外还有个 `a` 是 adapted 自适应。
4. 两种选项：`box` 和 `line`，即要么生成结果是个「框框」，要么就是条线。
5. 最后一个部分是风格样式，是个数字，属于可选。 就是那些框框或线条是什么样式的，实线啊还是虚线什么的。这有 22 种 box 和 17 种 line 样式可选：[comment-box.nvim-catalog](https://github.com/LudoPinelli/comment-box.nvim#the-catalog)。

> [!info] comment-box 相关资料
> 
> [comment-box.nvim-doc](https://github.com/LudoPinelli/comment-box.nvim/blob/main/doc/comment-box.txt)

#### todo-comments

[todo-comments.nvim](https://github.com/folke/todo-comments.nvim) 高亮注释关键字。

![todo-comments screenshot](https://user-images.githubusercontent.com/292349/118135272-ad21e980-b3b7-11eb-881c-e45a4a3d6192.png)

```lua
{
	"folke/todo-comments.nvim",
	dependencies = {"nvim-lua/plenary.nvim"},
	lazy = true,
	event = {"BufReadPost", "BufAdd"},
	config = function()
		require("todo-comments").setup({})
	end
}
```

---

### formatter.nvim

[formatter.nvim](https://github.com/mhartington/formatter.nvim) 格式化插件。

```lua
{
	"mhartington/formatter.nvim",
	config = function()
		require("formatter").setup(
			{
				filetype = {
					c = {
						require("formatter.filetypes.c").clangformat
					},
					lua = {
						require("formatter.filetypes.lua").luafmt
					},
					go = {
						require("formatter.filetypes.go").gofmt
					},
					cpp = {
						require("formatter.filetypes.cpp").clangformat
					},
					rust = {
						require("formatter.filetypes.rust").rustfmt
					},
					sh = {
						require("formatter.filetypes.sh").shfmt
					},
					vue = {require("formatter.filetypes.vue").prettier},
					css = {require("formatter.filetypes.css").prettier},
					html = {require("formatter.filetypes.html").prettier},
					javascript = {require("formatter.filetypes.javascript").prettier},
					typescript = {require("formatter.filetypes.typescript").prettier},
					markdown = {require("formatter.filetypes.markdown").prettier},
					json = {require("formatter.filetypes.json").prettier},
					less = {
						function()
							return {
								exe = "prettier",
								args = {"--stdin-filepath", vim.fn.shellescape(vim.api.nvim_buf_get_name(0))},
								stdin = true
							}
						end
					},
					python = {
						-- require("formatter.filetypes.python").autopep8
						require("formatter.filetypes.python").black
						-- require("formatter.filetypes.python").ruff
					},
					["*"] = {
						-- filetype
						-- 去除尾部空白
						require("formatter.filetypes.any").remove_trailing_whitespace
					}
				} --filetype
			}
		)
	end
}

```

### <span id="nvim_plugins_snippets">Snippet 插件</span>

#### nvim-snippy

[nvim-snippy](https://github.com/dcampos/nvim-snippy) 是一个支持 [vim-snippets](vim_plugin.md#vimplugin_snippets_vimsnippets) 的 snippet 插件。

如果只使用 vim-snippets 作为 snippet 的「仓库」，可以使用这个插件。如果想要使用多种 snippet 格式，建议换更强大的 [LuaSnip](#LuaSnip)。
> [!tip] 
> 
> 实话，nvim-snippy 真的很轻巧，比 [LuaSnip](#LuaSnip) 轻量级多了，如果对于自定义代码片段要求不太高，完全可以用它替代重量级的 luasnip。

当前如果使用 [nvim-cmp](#nvim-cmp)，还得再加个「桥接器」：[cmp-snippy](https://github.com/dcampos/cmp-snippy)，所以得在 `dependencies` 中把 nvim-snippy 和 cmp-snippy 都加上。

使用 nvim-cmp 配置：

1. 配依赖
```lua
dependencies = {
            "neovim/nvim-lspconfig",
            "hrsh7th/cmp-nvim-lsp",
            "hrsh7th/cmp-buffer",
            "hrsh7th/cmp-path",
            "hrsh7th/cmp-calc",
            "hrsh7th/cmp-cmdline",
            -- 使用 nvim-snippy
            "dcampos/nvim-snippy",
            "dcampos/cmp-snippy",
            "onsails/lspkind.nvim"
        }
```

2. `snippet` 配置项
```lua
snippet = {
	expand = function(args)
		-- 使用 nvim-snippy
		require "snippy".expand_snippet(args.body)
	end
}
```

3. `source` 配置项，配置数据源
```lua
sources = cmp.config.sources(
{
	{name = "nvim_lsp"},
	-- {name = "luasnip"},
	{name = "snippy"},
	{name = "path"},
	{name = "calc"}
}
```

4. `vim_item.menu` 配置项，这个配置项主要用来设置补全菜单侯选项的
```lua
formatting = {
	format = lspkind.cmp_format(
		{
			-- options: 'text', 'text_symbol', 'symbol_text', 'symbol'
			mode = "symbol_text",
			maxwidth = 50,
			ellipsis_char = "...",
			before = function(entry, vim_item)
				vim_item.menu =
					({
					buffer = "[Buffer]",
					nvim_lsp = "[LSP]",
					-- luasnip = "[LuaSnip]",
					snippy = "[nvim-snippy]",
					nvim_lua = "[Lua]"
					-- latex_symbols = "[LaTeX]",
				})[entry.source.name]
				return vim_item
			end
		}
	)
}
```
> [!info] 
> 
> `vim_item_menu` 中的 `snippy` 是 [List of sources · hrsh7th/nvim-cmp](https://github.com/hrsh7th/nvim-cmp/wiki/List-of-sources) 规定好的，每个 source 源都给了一个标识名。而后面的值，即方括号中的值，是补全菜单候选项中第个侯选项的数据源名称，可以自定义 -- 你写什么它就会显示什么。

#### LuaSnip

[LuaSnip](https://github.com/L3MON4D3/LuaSnip) 一个强大的 snippet 插件。

##### 加载器

> LuaSnip 能够加载不同格式的代码片段，包括成熟的 Vscode 和 Snipmate 格式，以及用 Lua 编写片段的纯 Lua 文件。

> [!info] 
> 
> 称 「snipmate 格式」，其实指的是 [vim-snippets](vim_plugin.md#vimplugin_snippets_vimsnippets) 这个 snippet 库。只不过在「传统」[Vim](Vim_Note.md) 中使用这个库的两大 snippet 引擎：[Ultisnips](vim_plugin.md#vimplugin_snippets_ultisnips) 和 [SnipMate](vim_plugin.md#vimplugin_snippets_snipmate)，而 Ultisnips 是用 [Python](../Python/Python_Note.md) 写的，snipmate 是使用纯 [vimscript](Vimscript_Note.md) 写的，而 vim-snippets 库同样也是 vimscript 写的，所以用它来指代 vim-snippets 库更合适。
>
> 而「VSCode 格式」指的是是 [Friendly-snippets](#Friendly-snippets) 这个 snippet 库。

```lua
require("luasnip.loaders.from_snipmate").load()
-- 指定加载路径
require("luasnip.loaders.from_snipmate").lazy_load({paths = "~/.config/nvim/snippets"})
-- or relative to the directory of $MYVIMRC
require("luasnip.loaders.from_snipmate").lazy_load({paths = "./snippets"})

```

###### 加载 [vim-snippets 库](vim_plugin.md#vimplugin_snippets_vimsnippets)

```lua
 dependencies = {
	"honza/vim-snippets",
	config = function()
		-- 加载 vim-snippets 库
		require("luasnip.loaders.from_snipmate").load()

	end
},
```

以 `dependencies` 方式安装 [vim-snippets](vim_plugin.md#vimplugin_snippets_vimsnippets)，安装位置是在：`~/.local/share/nvim/lazy/vim-snippets/`。所以如果要定位其 snippets，就应该是在 `~/.local/share/nvim/lazy/vim-snippets/snippets`。

> [!tip] 
> 
> LuaSnip 配置可以参考 [LazyVim](https://github.com/LazyVim/LazyVim/blob/main/lua/lazyvim/plugins/coding.lua) 的配置。

###### 加载 [Friendly-snippets 库](#Friendly-snippets)

```lua
dependencies = {
	"rafamadriz/friendly-snippets",
	config = function()
		-- 加载 rafamadriz/friendly-snippets 库
		require("luasnip.loaders.from_vscode").load()
	end
}
```

> [!info] 
> 
> [加载 [vim-snippets 库](vim_plugin.md vimplugin_snippets_vimsnippets)](#加载%20[vim-snippets%20库](vim_plugin.md%20vimplugin_snippets_vimsnippets)) 与 [加载 [Friendly-snippets 库]( Friendly-snippets)](#加载%20[Friendly-snippets%20库](%20Friendly-snippets)) 区别不太大，主要是库的代码片段有差异，另外，使用 [Friendly-snippets](#Friendly-snippets) 性能会好点，因为 Friendly-snippets 是使用 [Lua](../Lua/Lua_Note.md) 写的，加载时会快一点。

##### snippet 插件相关资料

* [LuaSnip 中文文档](https://zjp-cn.github.io/neovim0.6-blogs/nvim/luasnip/doc1.html)
* [Luasnip使用手册（上） - 知乎](https://zhuanlan.zhihu.com/p/637854706)
* [从零开始的Neovim配置--第2期：LuaSnip插件用法及其基本配置](https://www.bilibili.com/video/BV18g4y1V7qT)
* [自定义代码段LuaSnip入门](https://www.bilibili.com/video/BV1iL4y1B7gH)
* [【AstroNvim】第四期：LuaSnip](https://www.bilibili.com/video/BV1Lv421r7Sv)

#### Friendly-snippets

[friendly-snippets](https://github.com/rafamadriz/friendly-snippets) 跟 [vim-snippets](vim_plugin.md#vimplugin_snippets_vimsnippets) 一样是 snippet 库。

---

### <span id="nvim_plugins_lsp">LSP 插件</span>

#### lspconfig

neovim 本就已经内置了 LSP Client，只是配置麻烦，所以官方还弄了个插件：[nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) 来简化配置。

> [!info] 相关资料
> 
> * [NeoVim Builtin LSP的基本配置](https://xfyuan.github.io/2021/02/neovim-builtin-lsp-basic-configuration/)
> * [nvim-lspconfig server configuration doc](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md)
> * [Language specific plugins · neovim/nvim-lspconfig Wiki · GitHub](https://github.com/neovim/nvim-lspconfig/wiki/Language-specific-plugins)

neovim 并没有自动补全功能，它的补全是通过 `omnifunc` 绑定来实现的，代码去 [GitHub - neovim/nvim-lspconfig: Quickstart configs for Nvim LSP](https://github.com/neovim/nvim-lspconfig?tab=readme-ov-file#suggested-configuration)。

如果需要自动补全，就得需要第三方插件，官方建议的是：[nvim-cmp](#nvim-cmp)。

配置：

```lua
-- nvim-lspconfig
{
	"neovim/nvim-lspconfig",
	lazy = true,
	event = {"BufReadPre", "BufNewFile"},
	-- event = { "CursorHold", "CursorHoldI" },
	config = function()
		local lspconfig = require("lspconfig")

		local capabilities = vim.lsp.protocol.make_client_capabilities()
		capabilities.textDocument.completion.completionItem.snippetSupport = true

		-- local capabilities = require("cmp_nvim_lsp").default_capabilities()
		lspconfig.clangd.setup {}
		lspconfig.bashls.setup {}
		lspconfig.lua_ls.setup {}

		-- html css typescript
		lspconfig.html.setup {
			capabilities = capabilities
		}
		lspconfig.cssls.setup {
			capabilities = capabilities
		}
		lspconfig.tsserver.setup {}

		-- golang
		lspconfig.gopls.setup {}
		-- java
		-- lspconfig.jdtls.setup{ capabilities = capabilities }
		lspconfig.jdtls.setup {}

		-- python
		lspconfig.pyright.setup {
			settings = {
				pyright = {
					-- Using Ruff's import organizer
					disableOrganizeImports = true
				},
				python = {
					analysis = {
						-- Ignore all files for analysis to exclusively use Ruff for linting
						ignore = {"*"}
					}
				}
			}
		}

		-- local on_attach = function(client, bufnr)
		--     if client.name == "ruff_lsp" then
		--         -- Disable hover in favor of Pyright
		--         client.server_capabilities.hoverProvider = false
		--     end
		-- end

		-- 使用 ruff 或 ruff-lsp 来作python分析诊断
		-- lspconfig.ruff.setup {}
		lspconfig.ruff_lsp.setup {}

		-- ruby
		lspconfig.solargraph.setup {
			-- root_dir = function(fname)
			root_dir = function()
				return vim.fn.getcwd()
			end,
			settings = {
				solargraph = {}
			}
		}

		-- markdown
		-- lspconfig.marksman.setup {}
		lspconfig.markdown_oxide.setup {
			-- capabilities = capabilities,
			root_dir = function()
				return vim.fn.getcwd()
			end,
			option = {
				keyword_pattern = [[\(\k\| \|\/\|#\)\+]]
			}
		}

		-- rust
		lspconfig.rust_analyzer.setup {
			settings = {
				["rust-analyzer"] = {}
			}
		}

		--
		-- vim.api.nvim_create_autocmd(
		--     {"TextChanged", "InsertLeave", "CursorHold", "LspAttach"},
		--     {
		--         buffer = bufnr,
		--         callback = vim.lsp.codelens.refresh
		--     }
		-- )

		-- trigger codelens refresh
		-- vim.api.nvim_exec_autocmds("User", {pattern = "LspAttached"})
	end --config
} --nvim-lspconfig

```

##### python lsp

python lsp 上是使用 [pyright](Vim_LSP_Complete.md#pyright)+[ruff-lsp](Vim_LSP_Complete.md#ruff-lsp) 的组合。

pyright 是用来补全代码，而 ruff-lsp 是用来当 linter 用的。具体配置如下：

```lua
-- python
lspconfig.pyright.setup {
	settings = {
		pyright = {
			-- Using Ruff's import organizer
			disableOrganizeImports = true
		},
		python = {
			analysis = {
				-- Ignore all files for analysis to exclusively use Ruff for linting
				ignore = {"*"}
			}
		}
	}
}

-- local on_attach = function(client, bufnr)
--     if client.name == "ruff_lsp" then
--         -- Disable hover in favor of Pyright
--         client.server_capabilities.hoverProvider = false
--     end
-- end

-- 使用 ruff 或 ruff-lsp 来作python分析诊断
-- lspconfig.ruff.setup {}
lspconfig.ruff_lsp.setup {}
```

#### lspsaga.nvim

[lspsaga.nvim](https://github.com/nvimdev/lspsaga.nvim) 这是基于 neovim 内置 lsp 接口的轻量级 LSP 插件。

安装：

```lua
{
	'nvimdev/lspsaga.nvim',
    config = function()
        require('lspsaga').setup({})
    end,
}
```

> [!info] lspsaga 文档
> 
> * [lspsaga doc](https://nvimdev.github.io/lspsaga/)

#### trouble

[trouble.nvim](https://github.com/folke/trouble.nvim) 一个诊断相关的插件。它能在下半部显示相关诊断信息，这跟 [VSCode](../Editors/Editors_Note.md#editors_vscode) 等编辑器类似。

安装配置：

```lua
{
	"folke/trouble.nvim",
	dependencies = {"nvim-tree/nvim-web-devicons"},
	lazy = true,
	event = {"BufReadPost"},
	config = function()
		-- require("trouble").setup({
		-- })
		vim.keymap.set(
			"n",
			"<leader>xx",
			function()
				require("trouble").toggle()
			end
		)
		vim.keymap.set(
			"n",
			"<leader>xw",
			function()
				require("trouble").toggle("workspace_diagnostics")
			end
		)
		vim.keymap.set(
			"n",
			"<leader>xd",
			function()
				require("trouble").toggle("document_diagnostics")
			end
		)
		vim.keymap.set(
			"n",
			"<leader>xq",
			function()
				require("trouble").toggle("quickfix")
			end
		)
		vim.keymap.set(
			"n",
			"<leader>xl",
			function()
				require("trouble").toggle("loclist")
			end
		)
		vim.keymap.set(
			"n",
			"gR",
			function()
				require("trouble").toggle("lsp_references")
			end
		)
	end
}
```

> [!info] 
> 
> 这插件配置主要就是配些快捷键，能快速查看相关的诊断信息。

---

### <span id="nvim_plugins_completion">补全插件</span>

#### nvim-cmp

[nvim-cmp](https://github.com/hrsh7th/nvim-cmp/) 是一个补全框架插件。

##### 补全源

补全插件，本身是没有提供补全数据的，它补全的数据都是由补全源提供的。而补全源来源各异，有来自输入的缓存（Buffer），也有来自 [LSP](Vim_LSP_Complete.md)，或是来自 snippet 及其他各种补全插件，比如路径补全插件什么的，反正补全数据的来源五花八门，就看补全框架支持到什么程度。

在 cmp 中，是在 `sources` 配置节点指定各种补全源。

```lua
sources = cmp.config.sources({
	{ name = "nvim_lsp" },
	{ name = "luasnip" },
	{ name = "path" },
	{ name = "calc" },
},{
	{ name = "buffer", keyword_length = 3 },
})
```

这里就配了四个补全数据源，各源使用大括号及逗号区隔开。其中 `name` 是补全源的名称，是 cmp 规定好的，可以在 [官方wiki](https://github.com/hrsh7th/nvim-cmp/wiki) 中的 [补全源列表](https://github.com/hrsh7th/nvim-cmp/wiki/List-of-sources) 查询到。`keyword_length` 选项，是触发的字符数，没有配置默认是 `1`，就是输入一个字符就触发补全，出现补全候选菜单。

##### snippet 配置

nvim 中最流行的 snippet 插件就是 [LuaSnip](#LuaSnip)。而 cmp 与为 luasnip 写了个「桥接」插件：[Site Unreachable](https://github.com/saadparwaiz1/cmp_luasnip)。

当前对于 luasnip 作为 cmp 的 [补全源](#补全源) 之一，也有其「专属配制」。

```lua
snippet = {
	expand = function(args)
		require'luasnip'.lsp_expand(args.body)
	end,
},

sources = cmp.config.sources({
	{ name = "nvim_lsp" },
	{ name = "luasnip" },
	{ name = "path" },
	{ name = "calc" },
},{
	{ name = "buffer", keyword_length = 3 },
})
```

`sources` 不用说，就是配 [补全源](#补全源) 的，但对于 snippet 作为补全源之一，又有专门的配置节点：`snippet`。这个配置节点中有个 `expand` 的配置项，就是指定 snippet「展开」的函数。这跟着 cmp_luasnip 官方上的文档给的配置抄就行了：[https://github.com/saadparwaiz1/cmp_luasnip#cmp_luasnip](https://github.com/saadparwaiz1/cmp_luasnip#cmp_luasnip)。

##### 快捷键设置

快捷键设置，最核心的就是三个「向下选择」、「向上选择」和「确认」。

摘抄了 [官方文档](https://github.com/hrsh7th/nvim-cmp/blob/main/doc/cmp.txt) 的三段：

```txt
*cmp.select_next_item* (option: { behavior = cmp.SelectBehavior, count = 1 })
  Select the next item. Set count with large number to select pagedown.
  `behavior` can be one of:
  - `cmp.SelectBehavior.Insert`: Inserts the text at cursor.
  - `cmp.SelectBehavior.Select`: Only selects the text, potentially adds ghost_text at
    cursor.
```

```txt
*cmp.select_prev_item* (option: { behavior = cmp.SelectBehavior, count = 1 })
  Select the previous item. Set count with large number to select pageup.
  `behavior` can be one of:
  - `cmp.SelectBehavior.Insert`: Inserts the text at cursor.
  - `cmp.SelectBehavior.Select`: Only selects the text, potentially adds ghost_text at
    cursor.
```

> [!info] 文档解析
> 
> 向下向上选择，默认效果是光标所在文字随着侯选项向下向上移动变化而即刻发生变动，这好处是对于非常确认输入会出现可选的侯选项情况下，提高输入流畅度。
> 
> 但如果是不确定输入后是否会出现可预期的侯选项，但又因为在侯选项菜单中，移动**选择游标**「试着」选择，这时光标所在的文字已经随着游标所处的侯选项的改变而发生更改，而这更改都不是用户想要的，即侯选项菜单中所有选项均非目标选项，但光标所在的文字已更改，这非常不符合需求。这就要求在设置相应快捷键时，对「选择」进行更细腻的设置。
> 
> `cmp.select_prev_item` 就是「**选择游标**」，而 `behavior` 就是设置它的行为，即它上下移动会发生什么效果。
> 
> 文档中就两个设置值：`cmp.SelectBehavior.Select` 和 `cmp.SelectBehavior.Insert` 。前者就是只是「选择」（`Select`），光标所处的文字不会发生任何变化，除非你再按「确认键」，比如回车；而后者，顾名思义，`Insert` 就意味着，游标移动到哪个侯选项，光标所处的文字就会变成侯选项的值，就如同 `insert` 了一般。

```txt
*cmp.confirm* (option: cmp.ConfirmOption, callback: function)
  Accepts the currently selected completion item.
  If you didn't select any item and the option table contains `select = true`,
  nvim-cmp will automatically select the first item.

  You can control how the completion item is injected into
  the file through the `behavior` option:

  `behavior=cmp.ConfirmBehavior.Insert`: inserts the selected item and
    moves adjacent text to the right (default).
  `behavior=cmp.ConfirmBehavior.Replace`: replaces adjacent text with
    the selected item. 
```

> [!info] 
> 
> 这个是确认键的设置。同样也有两设置：
> 
> * `cmp.ConfirmBehavior.Insert`，就是把侯选项的文本值「追加」到现有光标的右边。
> * `cmp.ConfirmBehavior.Replace`，这是将侯选项的文本值，替换光标所在那个文本值。

下面这是不与 snippet 插件「关联性」地对选择游标操作及确认键设置：

```lua

-- No snippet plugin
["<Tab>"] = cmp.mapping(function(fallback)
  if cmp.visible() then
	cmp.select_next_item({ behavior = cmp.SelectBehavior.Select })
  else
	fallback()
  end
end, {
  "i",
  "s",
}),

["<S-Tab>"] = cmp.mapping(function(fallback)
  if cmp.visible() then
	cmp.select_prev_item( { behavior = cmp.SelectBehavior.Select })
  else
	fallback()
  end
end, {
  "i",
  "s",
}),


["<CR>"] = cmp.mapping({ 
	i = function(fallback)
	 if cmp.visible() and cmp.get_active_entry() then
	   cmp.confirm({ behavior = cmp.ConfirmBehavior.Replace, select = false })
	 else
	   fallback()
	 end
	end,
	s = cmp.mapping.confirm({ select = true }),
	c = cmp.mapping.confirm({ behavior = cmp.ConfirmBehavior.Replace, select = true }),
}),



```

> [!info] 相关资料
> 
> * [nvim-cmp Wiki](https://github.com/hrsh7th/nvim-cmp/wiki)
> * [nvim-cmp doc](https://github.com/hrsh7th/nvim-cmp/blob/main/doc/cmp.txt)
> * [Neovim 内置 LSP 配置 (二)：自动代码补全 - 知乎](https://zhuanlan.zhihu.com/p/445331508)

---

### <span id="nvim_plugins_finder">搜索器</span>

#### fzf-lua

[fzf-lua](https://github.com/ibhagwan/fzf-lua) 这是使用 fzf 进行文件搜索的搜索器。

安装配置：

```lua
{
	"ibhagwan/fzf-lua",
	dependencies = {"nvim-tree/nvim-web-devicons"},
	event = {"VimEnter"},
	config = function()
		require("fzf-lua").setup(
			{
				-- file
				vim.keymap.set("n", "<c-P>", "<cmd>lua require('fzf-lua').files()<CR>", {silent = true}),
				-- buffer
				vim.keymap.set("n", "<c-e>", "<cmd>lua require('fzf-lua').buffers()<CR>", {silent = true}),
				-- path
				vim.keymap.set(
					{"n", "v", "i"},
					"<C-x><C-f>",
					function()
						require("fzf-lua").complete_path()
					end,
					{silent = true, desc = "Fuzzy complete path"}
				)
			}
		)
	end
}
```

就配了下几个快捷键：

* `Ctrl-p`：文件查找
* `Ctrl-e`：[buffer](Vim_Note.md#buffer) 查找
* `Ctrlx,Ctrl-f`：路径补全

#### telescope

[telescope.nvim](https://github.com/nvim-telescope/telescope.nvim) 是一个强大的搜索器。

![telescope screenshot](https://camo.githubusercontent.com/93ccc50f336bf712787f4872e237b8ed3ac99353d18d69500c931a6c608e6c12/68747470733a2f2f692e696d6775722e636f6d2f5454546a6136742e676966)

默认快捷键：

* `Ctrl+C`：关闭面板

##### Telescope 扩展

telescope 还能装扩展：[telescope extensions](https://github.com/nvim-telescope/telescope.nvim/wiki/Extensions)

##### 配置 

telescope 主配置文件：

```lua
 {
	"nvim-telescope/telescope.nvim",
	dependencies = {"nvim-lua/plenary.nvim"},
	event = {"VimEnter"},
	config = function()
		-- 内置功能
		local builtin = require("telescope.builtin")
		vim.keymap.set("n", "<leader>ff", builtin.find_files, {})
		vim.keymap.set("n", "<leader>fg", builtin.live_grep, {})
		vim.keymap.set("n", "<leader>fb", builtin.buffers, {})
		vim.keymap.set("n", "<leader>fh", builtin.help_tags, {})
		require("telescope").setup {
			-- 扩展设置
			extensions = {
				-- file-browser
				file_browser = {
					theme = "ivy"
				},
				-- fzf-native
				fzf = {
					fuzzy = true,
					override_generic_sorter = true,
					override_file_sorter = true,
					case_mode = "smart_case"
				},
			}
		}
		--加载扩展
		-- file_browser
		require("telescope").load_extension("file_browser")
		-- fzf-native
		require("telescope").load_extension("fzf")
	end
}
```

扩展放在另一个配置文件中：

```lua
-- file-browser
{
	"nvim-telescope/telescope-file-browser.nvim",
	dependencies = {"nvim-telescope/telescope.nvim", "nvim-lua/plenary.nvim"}
},

-- fzf-native
{
	"nvim-telescope/telescope-fzf-native.nvim",
	dependencies = {"nvim-telescope/telescope.nvim", "nvim-lua/plenary.nvim"},
	build = "make"
}

```

##### 相关资料

* [Vim/Neovim 全文检索插件 -- telescope.nvim - 知乎](https://zhuanlan.zhihu.com/p/609527018)
* [vim练级手册（七） ---telescope文件模糊搜索](https://command-z-z.github.io/2022/04/23/vim%E7%BB%83%E7%BA%A7%E6%89%8B%E5%86%8C%EF%BC%88%E4%B8%83%EF%BC%89-telescope%E6%96%87%E4%BB%B6%E6%A8%A1%E7%B3%8A%E6%90%9C%E7%B4%A2/)
* [打造neovim IDE第六期：telescope_bilibili](https://www.bilibili.com/video/BV1P34y1e7SF)
* [神级文件模糊搜索插件telescope_bilibili](https://www.bilibili.com/video/BV1r3411C7yx/)
* [NeoVim 从平凡到非凡 第4集：Telescope 模糊搜索_bilibili](https://www.bilibili.com/video/BV1u14y197AT)

---

### <span id="nvim_plugins_icons">图标</span>

#### lspkind.nvim

[lspkind.nvim](https://github.com/onsails/lspkind.nvim) 为 [补全插件](#补全插件) 的候选菜单中添加图标。

![lspkind.nvim screenshot](https://github.com/onsails/lspkind-nvim/raw/images/images/screenshot.png)

#### nvim-colorizer

[nvim-colorizer.lua](https://github.com/NvChad/nvim-colorizer.lua) 这是高亮颜色代码的小插件。

![nvim-colorizer screenshot](https://raw.githubusercontent.com/norcalli/github-assets/master/nvim-colorizer.lua-demo-short.gif)

简单配置：

```lua
{
	"NvChad/nvim-colorizer.lua",
	envent = {"BufPost"},
	enabled = true,
	config = function()
		require("colorizer").setup(
			{
				-- 允许显示的文件类型
				-- 还能单独谁每个文件类型设置不同的显示样式
				-- 如果没有设置样式，将使用默认样式设置，即 user_default_options选项设置的样式
				filetypes = {
					"css",
					"sass",
					"javascript",
					"html"
					-- html = {mode = "foreground"}
				},
				-- 默认显示样式设置
				user_default_options = {
					-- 高亮的样式
					-- background 代码背景色显示相应的颜色 这是默认
					-- foreground 代码字体颜色显示相应的颜色
					-- virtualtext 代码添加自定义虚拟文本以显示相应的颜色
					-- virtualtext 还能自定义不同的 Unicode 文字。默认是 virtualtext = "■"
					-- mode = "foreground"
					-- https://symbl.cc/ 或 https://www.fuhaoku.net/ 找图形
					-- virtualtext = "◉",
					virtualtext = "●",
					mode = "virtualtext"
				}
			}
		)
	end
}
```

在命令行模式，还能使用 `ColorizerToggle` 命令临时对颜色高亮功能进行开启和关闭。

---

### <span id="nvim_plugins_cursorword">高亮单词</span>

#### stcursorword

[stcursoword](https://github.com/sontungexpt/stcursorword) 是一个高亮光标所在单词的小插件。

经过使用 [nvim-cursorline](https://github.com/yamatsum/nvim-cursorline) 这插件会莫名其妙失效，而 stcursorword 这插件确正常工作。

简单配置：

```lua
 -- stcursorword
{
	"sontungexpt/stcursorword",
	event = "VeryLazy",
	enabled =true,
	config = function()
		require("stcursorword").setup(
			{
				min_word_length = 2,
				max_word_length = 50
			}
		)
	end
}
```

##### 类似插件

* [nvim-cursorword](https://github.com/xiyaowong/nvim-cursorword)
* [nvim-cursorline](https://github.com/yamatsum/nvim-cursorline)

#### sentiment

[sentiment.nvim](https://github.com/utilyre/sentiment.nvim) 是一个高亮光标所在区块成对括号的插件。

安装配置：

```lua
{
  "utilyre/sentiment.nvim",
  version = "*",
  lazy = true,
  event = {"BufReadPost", "BufAdd", "BufNewFile"},
  opts = {
    -- config
  },
  init = function()
    -- `matchparen.vim` needs to be disabled manually in case of lazy loading
    vim.g.loaded_matchparen = 1
  end,
}
```

---

### <span id="nvim_plugins_codenavi">代码导航</span>

#### nvim-navic

[nvim-navic](https://github.com/SmiteshP/nvim-navic) 是一个以「面包屑」形式显示代码结构的小插件。

![nvim-navic screenshot](https://user-images.githubusercontent.com/43147494/173186210-c8d689ad-1f8a-43cf-8125-127c7bd5be35.gif)

简单配置：

```lua

{
	"SmiteshP/nvim-navic",
	dependencies = "neovim/nvim-lspconfig",
	lazy = true,
	event = {"BufReadPost"},
	config = function()
		local navic = require("nvim-navic")

		vim.o.winbar = "%{%v:lua.require'nvim-navic'.get_location()%}"

		-- local on_attach = function(client, bufnr)
		--     if client.server_capabilities.documentSymbolProvider then
		--         navic.attach(client, bufnr)
		--     end
		-- end

		navic.setup(
			{
				lsp = {
					auto_attach = true,
					-- auto_attach = false,
					-- on_attach = on_attach,
					-- preference = {"clangd", "gopls"}
					preference = {"ruff-lsp", "solargraph", "typescript-language-server", "bash-language-server"}
				}
			}
		)
	end
}


```

> [!info] 
>
> 这插件是与 LSP 相关的，所以得依赖 [lspconfig](#lspconfig) 插件。
> 
> `vim.o.winbar = "%{%v:lua.require'nvim-navic'.get_location()%}"` 这句配置得加上，不然不生效。
>
> `preference` 这个配不配都行，这主要是对于一些有多个 [LSP](Vim_LSP_Complete.md) 时，指定优先使用的 LSP。

#### goto-preview

[goto-preview](https://github.com/rmagatti/goto-preview) 一个代码预览插件。

![goto-preview screenshot](https://github.com/rmagatti/readme-assets/raw/main/goto-preview-zoomed.gif)

安装配置：

```lua
{
	"rmagatti/goto-preview",
	lazy = true,
	event = {"BufReadPost"},
	config = function()
		require("goto-preview").setup {}
	end
}
```

不想自己配快捷键，就在配置里加 `default_mappings = true` 这句，使用默认定义的快捷键（[goto-preview mappings](https://github.com/rmagatti/goto-preview#%EF%B8%8F-mappings)）：

```lua
nnoremap gpd <cmd>lua require('goto-preview').goto_preview_definition()<CR>
nnoremap gpt <cmd>lua require('goto-preview').goto_preview_type_definition()<CR>
nnoremap gpi <cmd>lua require('goto-preview').goto_preview_implementation()<CR>
nnoremap gpD <cmd>lua require('goto-preview').goto_preview_declaration()<CR>
nnoremap gP <cmd>lua require('goto-preview').close_all_win()<CR>
nnoremap gpr <cmd>lua require('goto-preview').goto_preview_references()<CR>
```

还能对预览窗口大小设置：

```lua
width = 80; 
height = 15;
```

---

### <span id="nvim_plugins_mark">标记</span>

#### marks.nvim

[marks.nvim](https://github.com/chentoast/marks.nvim) 是一个 marks 增强插件。

![marks.nvim screenshot](https://github.com/chentoast/marks.nvim/raw/assets/marks-demo.gif)

配置就直接使用官方给的：

```lua
{
	"chentoast/marks.nvim",
	config = function()
		require("marks").setup(
			{
				-- whether to map keybinds or not. default true
				default_mappings = true,
				-- which builtin marks to show. default {}
				builtin_marks = {".", "<", ">", "^"},
				-- whether movements cycle back to the beginning/end of buffer. default true
				cyclic = true,
				-- whether the shada file is updated after modifying uppercase marks. default false
				force_write_shada = false,
				-- how often (in ms) to redraw signs/recompute mark positions.
				-- higher values will have better performance but may cause visual lag,
				-- while lower values may cause performance penalties. default 150.
				refresh_interval = 250,
				-- sign priorities for each type of mark - builtin marks, uppercase marks, lowercase
				-- marks, and bookmarks.
				-- can be either a table with all/none of the keys, or a single number, in which case
				-- the priority applies to all marks.
				-- default 10.
				sign_priority = {lower = 10, upper = 15, builtin = 8, bookmark = 20},
				-- disables mark tracking for specific filetypes. default {}
				excluded_filetypes = {},
				-- disables mark tracking for specific buftypes. default {}
				excluded_buftypes = {},
				-- marks.nvim allows you to configure up to 10 bookmark groups, each with its own
				-- sign/virttext. Bookmarks can be used to group together positions and quickly move
				-- across multiple buffers. default sign is '!@#$%^&*()' (from 0 to 9), and
				-- default virt_text is "".
				bookmark_0 = {
					sign = "⚑",
					virt_text = "hello world",
					-- explicitly prompt for a virtual line annotation when setting a bookmark from this group.
					-- defaults to false.
					annotate = false
				},
				mappings = {}
			}
		)
	end
}
```

其实以上都不需要配，真正必须要配的只有一个：`force_write_shada = true`。即强制写入 [shada](vim及neovim配置.md#shada)。如果没有这句配置，永远删除不了 marks，虽然当时无论通过 `dm` 命令还是原生的 `delmarks!` 命令，只能暂时性删除 mark 标记，一旦下一次再将读取这个文件，mark 依然还在。这就是的「持久化」问题了，这就涉及到 [shada](vim及neovim配置.md#shada) 文件。而 `force_write_shada` 就是设置是否强制将对 mark 的操作结果写入 `shada` 文件，保证了对 mark 的「持久化」生效。

> [!info] 相关资料
> 
> * [marks.nvim issues 13](https://github.com/chentoast/marks.nvim/issues/13)

默认快捷键：

* `dm-`：删除当前行所有的标记（一行可以「叠」多个标记）
* `dm<space>`：删除当前 buffer 所有标记
> [!tip]
>  
> 好像有 bug，设置了 `force_write_shada = true`，也还是没能永远删除 mark，只能暂时使用原生的 `:delmarks!`。
* `dm=`：删除光标下的标记
* `dmx`：删除 x 标记。
* `m]`：跳到下一个标记
* `m[`：跳到上一个标记
* `m:`：预览标记

> [!info] 
> 
> [marks.nvim mappins](https://github.com/chentoast/marks.nvim?tab=readme-ov-file#mappings)

---

### <span id="nvim_plugins_utils">小工具</span>

#### which-key

[which-key.nvim](https://github.com/folke/which-key.nvim) 是一个可以查看快捷键信息的插件。

![which-key screenshot](https://user-images.githubusercontent.com/292349/116439438-669f8d00-a804-11eb-9b5b-c7122bd9acac.png)

安装：

```lua
{
  "folke/which-key.nvim",
  event = "VeryLazy",
  init = function()
    vim.o.timeout = true
    vim.o.timeoutlen = 300
  end,
  opts = {
    -- your configuration comes here
    -- or leave it empty to use the default settings
    -- refer to the configuration section below
  }

```

`:WhichKey` 用来查询快捷键。

 `:WhichKey` 默认有两个参数：

1. 要查询的快捷键开头的字符串
2. [模式](Vim_Note.md#vim_mode) 字符，用来限定只显示某模式的快捷键

#### smartcolumn

[smartcolumn](https://github.com/m4xshen/smartcolumn.nvim) 一个智能的边界插件。

![smartcolumn screenshot](https://user-images.githubusercontent.com/74842863/219844450-37d96fe1-d15d-4aaf-ae57-1c6ce66d8cbc.gif)

#### colorful-winsep

[colorful-winsep.nvim](https://github.com/nvim-zh/colorful-winsep.nvim) 是高亮当前 [窗口（Window）](vim常用操作.md#op_normal_windows) 的小插件。

安装配置：

```lua
{
	"nvim-zh/colorful-winsep.nvim",
	lazy = true,
	event = {"WinNew"},
	config = function()
		require("colorful-winsep").setup({})
	end
}
```

#### LoremIpsum

[lorem.nvim](https://github.com/derektata/lorem.nvim) 是一个 loremipsum 插件。

最简配置：

```lua
{
	"derektata/lorem.nvim",
	event = "VeryLazy"
}
```

当然可以对句子长度作相关的配置：

```lua
require('lorem').setup({
  sentenceLength = "mixedShort",
  comma = 0.1
})
```
> [!info] 
> 
> `comma` 的值是逗号出现的频率（至少是 3 个单词一个逗号），值范围是 `0~1` 之间。`0` 是禁止逗号，`1` 是每**3**个单词就一个逗号。这配置项不能单独配，得跟 `sentenceLength` 一起配，如果不配 `setenceLength` 只有 `comma`，会报错。不知道算不算 bug。
> 

##### 命令

就一个命令，很简单：

`:LoremIpsum 数字或句子长度单词`

> [!example] 
> 
> * `:LoremIpsum 500`：生成一个 500 个单词的句子。默认句长长度是 `100`。
> * `:LoremIpsum short`：生成一个 3~20 句子的段落。

段落长度属性值，是随机生成的句子长度，每种属性值有一个生成长度范围，如 `short`，就是 `3~29`。具体参考官方说明：[Lorem.nvim README sethencelength-property](https://github.com/derektata/lorem.nvim#the-sentencelength-property)

#### smoothCursor

[SmoothCursor](https://github.com/gen740/SmoothCursor.nvim) 一个光标移动特效插件。

能在侧边栏使用小标志显示当前行。

安装：

```lua
{
	"gen740/SmoothCursor.nvim",
	lazy = true,
	event = {"BufReadPost", "BufAdd", "BufNewFile"},
	config = function()
		require("smoothcursor").setup()
	end
}
```

[SmoothCursor 相关命令](https://github.com/gen740/SmoothCursor.nvim#commands)

#### Cybu

[cybu](https://github.com/ghillb/cybu.nvim) 是一个快速切换 [buffer](Vim_Note.md#buffer) 的小插件。

「Cy」：「Cycle」；「bu」：「Buffer」，顾名思义，就是循环切换 Buffer。

通过使用这个插件，可以让 nvim 实现像 [VSCode](../Editors/Editors_Note.md#editors_vscode) 使用 `Ctrl+Tab` 快速标签一样，快速切换 [buffer](Vim_Note.md#buffer)。

![cybu screenshot](https://user-images.githubusercontent.com/35503959/169406683-6fb0c4dd-2083-4b9b-87b2-3928da81d472.gif)

安装：

```lua

```

---

### 插件推荐清单

一些网站整理的 nvim 插件推荐集。

* [nvimluau.dev](https://nvimluau.dev/)
* [Track Awesome Neovim (rockerBOO/awesome-neovim)](https://www.trackawesomelist.com/rockerBOO/awesome-neovim/)
* [awesome-neovim - Github](https://github.com/rockerBOO/awesome-neovim)

---

### <span id="nvim_plugins_obasic">旧基础插件</span>

基础插件，大概围绕什么注释、光标样式等编辑基础的功能增加。

有些插件已经不适用于使用 [Lua](../Lua/Lua_Note.md) 的 nvim 了。

* [preservim/nerdcommenter](https://github.com/preservim/nerdcommenter)
* [tpope/vim-surround](https://github.com/tpope/vim-surround)
* [vim-cursorword](https://github.com/itchyny/vim-cursorword)
* ~~[machakann/vim-highlightedyank](https://github.com/machakann/vim-highlightedyank)~~
* [jszakmeister/vim-togglecursor](https://github.com/jszakmeister/vim-togglecursor)
* [auto-pairs](https://github.com/jiangmiao/auto-pairs)
* [wellle/targets.vim](https://github.com/wellle/targets.vim)
* [terryma/vim-expand-region](https://github.com/terryma/vim-expand-region)

---

## <span id="nvim_colourscheme">配色</span>

如果使用配色 [插件](#插件) 来安装相应的配色，可以直接在那具配色插件里配置上：`vim.cmd.colorscheme "配色名"`，然后通过 `enabled` 对这配色插件进行开关，只有开启这配色插件，就能直接启用相应的配色。这样免去了，再另新建配置文件对配色进行启用或禁用。

示例如下：

```lua
{
	"catppuccin/nvim",
	name = "catppuccin",
	priority = 1000,
	enabled = false,
	-- enabled = true,
	config = function()
		require("catppuccin").setup(
			{
				-- vim.cmd.colorscheme "catppuccin"
				-- vim.cmd.colorscheme "catppuccin-latte"
				-- vim.cmd.colorscheme "catppuccin-frappe"
				-- vim.cmd.colorscheme "catppuccin-macchiato"
				vim.cmd.colorscheme "catppuccin-mocha"
			}
		)
	end
}
```

### material

[material.nvim](https://github.com/marko-cerovac/material.nvim)

![material screenshot 1](https://user-images.githubusercontent.com/76592799/163740695-3c34201c-7ae4-482f-9548-53d08701bdd5.png)

material 这个颜色主题，其中有不同的配色，得通过 `vim.g.material_style` 来设置：

```lua
# 这个设置必须放在colorscheme之前，不然不会生效，就只能用material默认配色
vim.g.material_style = "darker"
vim.cmd.colorscheme "material"
```

### gruvbox

[GitHub - ellisonleao/gruvbox.nvim: Lua port of the most famous vim colorscheme](https://github.com/ellisonleao/gruvbox.nvim)

![gruvbox screenshot](https://camo.githubusercontent.com/8dfd1f3d917727229318863a5c11d9f23e601332f572d646c054c374eb251b4a/68747470733a2f2f692e706f7374696d672e63632f667933746e4746742f67727576626f782d7468656d65732e706e67)

```lua
# 就俩配色：light和dark，这行设置一样得放在colorscheme设置之前
-- vim.o.background = "dark"
vim.o.background = "light"
vim.cmd.colorscheme "gruvbox"
```

有三种对比度：`"hard"`、`"soft"` 和默认 `""`

```lua
{ 
	"ellisonleao/gruvbox.nvim", 
	priority = 1000 , 
	config = true, 
	config = function()
		require("gruvbox").setup({})
	end,
}
```

### catppuccin

[catppuccin](https://github.com/catppuccin/nvim) 又是经典的配色。

![catppuccin screenshot](https://user-images.githubusercontent.com/56817415/213472445-091e54fb-091f-4448-a631-fa6b2ba7d8a5.png)

有四个配色，其中 `latte` 是浅色：

```lua
{ 
	"catppuccin/nvim", 
	name = "catppuccin", 
	priority = 1000,
	config = function()
		require("catppuccin").setup({
			-- latte frappe macchiato mocha
			-- flavour = "mocha",
			-- flavour = "latte",
			-- flavour = "frappe",
			flavour = "macchiato",
		})
	end,
}

```

#### Base16

[base16-nvim](https://github.com/RRethy/base16-nvim) 这是一个配色集，有大量的流行配色。

```lua
{
	"RRethy/base16-nvim",
	enabled = true,
	-- enabled = false,
	config = function()
		require("base16-colorscheme").with_config(
			{
				-- telescope = true,
				-- indentblankline = true,
				-- notify = true,
				-- ts_rainbow = true,
				cmp = true,
				-- illuminate = true,
				-- dapui = true,

				-- 设置配色
				-- 多种配色：https://github.com/RRethy/base16-nvim
				-- vim.cmd.colorscheme "base16-ayu-mirage"
				-- vim.cmd.colorscheme "base16-catppuccin-macchiato"
				-- vim.cmd.colorscheme "base16-catppuccin-mocha"
				-- vim.cmd.colorscheme "base16-monokai"
				-- vim.cmd.colorscheme "base16-materia"
				-- vim.cmd.colorscheme "base16-material"
				-- vim.cmd.colorscheme "base16-material-darker"
				-- vim.cmd.colorscheme "base16-material-palenight"
				-- vim.cmd.colorscheme "base16-ocean"
				-- vim.cmd.colorscheme "base16-onedark"
				-- vim.cmd.colorscheme "base16-tokyo-night-dark"
				-- vim.cmd.colorscheme "base16-tomorrow-night"
				-- vim.cmd.colorscheme "base16-gruvbox-dark-medium"
				-- vim.cmd.colorscheme "base16-gruvbox-material-dark-medium"
				vim.cmd.colorscheme "base16-everforest"
			}
		)
	end
} -- base16-nvim
```

---

## nvim 整合套件

* [LazyVim](https://github.com/LazyVim)
* [AstroNvim](https://astronvim.com/)
* [LunarVim](https://github.com/lunarvim/lunarvim)

---

## neovim 相关资料

### 各种配置资料

* [nvim lua 指南 中文版](https://github.com/glepnir/nvim-lua-guide-zh)
* [Neovim插件推荐&配置 - 哔哩哔哩](https://www.bilibili.com/read/cv22495061/)
* [ADkun/lvim-config-suggest](https://github.com/ADkun/lvim-config-suggest/blob/main/README.md)
* [NeoVim 插件推荐 - inner.ren](https://blog.innei.ren/nvim-plugin-recommend?locale=zh)
* [nvim-lua-guide-zh: neovim指导nvim-lua-guide-zh](https://gitee.com/zhengqijun/nvim-lua-guide-zh)
* [详解nvim内建LSP体系与基于nvim-cmp的代码补全体系 - 知乎](https://zhuanlan.zhihu.com/p/643033884)
* [awesome-newvim](https://github.com/rockerBOO/awesome-neovim)
* [从零开始配置vim(21)——lsp简介与treesitter 配置-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/2127555)
* [Neovim 主题配置 - 知乎](https://zhuanlan.zhihu.com/p/438382701)
* [nvim代码格式化插件formatter.nvim](https://blog.csdn.net/lxyoucan/article/details/120411901)
* [懒惰的Neovim](https://xfyuan.github.io/2023/02/lazy-neovim/)

### 各种文档

* [Lua-guide - Neovim docs](https://neovim.io/doc/user/lua-guide.html#lua-guide)

---

## 参考配置

* [xfyuan/nvim](https://github.com/xfyuan/nvim)
* [LintaoAmons/CoolStuffes](https://github.com/LintaoAmons/CoolStuffes)
* [luyuhuang](https://github.com/luyuhuang/nvim)

---

## 其他笔记

* [Vim笔记](Vim_Note.md)
* [vim及neovim配置](vim及neovim配置.md)
* [vim插件](vim_plugin.md)
* [Vim视频清单](Vim_Videos.md)
* [vim常用操作](vim常用操作.md)
* [vimscript笔记](Vimscript_Note.md)
* [LSP笔记](../Protocols/LSP_Note.md)
