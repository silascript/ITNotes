---
aliases: []
tags:
  - editor
  - vim
  - plugin
  - lsp
  - markdown
  - vim-plugin
  - list
created: 2023-01-30 11:19:11
modified: 2024-03-25 03:34:11
---

# vim 常用插件清单

---

## 目录

* [插件管理器](#vimplugin_mgmt)
	* [vim-Plug](#vimplugin_mgmt_plug)
	* [plugpac](#vimplugin_mgmt_plugpac)
* [插件列表](#vimplugin_list)
	* [自动符号](#vimplugin_auto_pairs)
	* [snippets插件](#vimplugin_snippets)
	* [格式化插件](#vimplugin_format)
	* [注释插件](#vimplugin_comment)
		* [nerdcommentor](#vimplugin_nerdcommentor)
	* [状态栏插件](#vimplugin_statusline)
		* [airline](#vimplugin_sl_airline)
	* [文件类型图标](#vimplugin_filetype_icon)
	* [语法增强](#vimplugin_syntax)
		* [ployglot](#vimplugin_syn_ployglot)
			* [子插件](#vimplugin_syn_subplugin)
	* [符号操作](#vimplugin_operator)
	
	* [Git相关插件](#vimplugin_git)
	
	* [预览插件](#vimplugin_preview)
	
	* [小工具](#vimplugin_tools)
	  * [高亮](#vimplugin_hightlight)
		* [vim-highlightedyank](#vimplugin_hlyank)
	  * [mark 相关](#vimplugin_mark)
		* [vim-signature](#vimplugin_mk_signature)
	  * [浏览器插件](#vimplugin_browser)
	    * [open-browser](#vimplugin_browser_openbrowser)
	    * [browser-github](#vimplugin_browser_github)
	* [MarkDown相关](#vimplugin_markdown)
	  * [Markdown 预览插件](#vimplugin_md_privew)
	    * [markdown-preview](#vimplugin_md_privew_1)
	    * [vim-markdown-preview](#plugin_md_privew_2)
	    * [preview-markdown.vim](#vimplugin_md_privew_3)
	  * [Markdown 表格相关的插件](#vimplugin_md_table)
	   * [VIM Table Mode](#vimplugin_md_table_1)
	   * [markdowntable](#vimplugin_md_table_2)
* [关于LSP及补全](#lsp_complete)
* [相关笔记](#相关笔记)

---

## <span id="vimplugin_mgmt">插件管理器</span>

### <span id="vimplugin_mgmt_plug">vim-Plug</span>

[vim-plug](https://github.com/junegunn/vim-plug) 是一款使用非常普遍的 [Vim](Vim_Note.md) 插件管理器。

 #vim-plug

windows 下装 [Plug 插件](https://github.com/junegunn/vim-plug)

```po
md ~\AppData\Local\nvim\autoload
$uri = 'https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
(New-Object Net.WebClient).DownloadFile(
  $uri,
  $ExecutionContext.SessionState.Path.GetUnresolvedProviderPathFromPSPath(
    "~\AppData\Local\nvim\autoload\plug.vim"
  )
)
```

其中 `~\AppData\Local\nvim\autoload\plug.vim`，可以自行定义下载安装 plug.vim 文件的目录，可以用官方给的，就在 `\AppData\Local\nvim\autoload` 下。如果是用 scoop 安装 neovim，可以放在 `current\share\nvim\runtime\autoload` 
 这个目录下，同样起效。

Linux vim：
```shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Linux neovim：
```shell
sh -c 'curl -fLo "${XDG_DATA_HOME:-$HOME/.local/share}"/nvim/site/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
```

#### vim-plug 插件配置

```vim
 call plug#begin()

 Plug 'vim-airline/vim-airline'

call plug#end()
```

> [!info] 
> 
> 如果 begin() 中不写具体插件安装地下，windows 下会装在 `C:\Users\用户名\AppData\Local\nvim\plugged` 这个目录下。
>
> 在 `begin` 与 `end` 之间是配置各种插件

写完后，重启 nvim 后，使用 `PlugInstall` 命令来执行安装。如果不使用某插件就在配置文件中注释掉，再执行 `PlugUpdate` 命令完成移除插件。

Plug 配置插件还可以 **按需加载**:

例子:

```shell
Plug 'othree/xml.vim',{'for':'xml'}
```

```shell
Plug 'google/vim-codefmt',{'on':['FormatCode','FormatLines']}
```

常用 **按需加载**：

| `do`  | Post-update hook，某些 vim 插件在完成安装或更新后，需要执行额外的操作，可以使用 do 选项指定具体的操作或函数 |
| ----- | ------------------------------------------------------------ |
| `on`  | 按需加载: vim 命令或 `<Plug>`-mappings                         |
| `for` | 按需加载: 文件类型                                           |

#### 延迟加载

使用 `{'on': []}` 可以禁止加载某插件，而使用自动命令来指定触发加载的条件，示例如下： #vim-autocmd 

```vim
Plug 'easymotion/vim-easymotion',{'on': []}
augroup load_easymotion
	autocmd!
	autocmd BufEnter * call plug#load('vim-easymotion') | autocmd! load_easymotion 
augroup END
```

> [!info] 
> 
> 上面的例子，就是先禁加载 easymotion 插件，然后使用 [自动命令](Vim_Note.md#自动命令)`autocmd` 来制定触发 easymotion 加载条件。
> 
> `autocmd` 后面跟的是 [事件](Vim_Note.md#events)（`event`）。可以查看官方文档：[VIM 中文帮助: 事件](https://yianwillis.github.io/vimcdoc/doc/autocmd.html#autocmd-events)
> 
> `plug#load()` 中是插件名，可以通过 `:PlugStatus` 命令查看。
> 
>> [!note] 相关资料
>> 
>> * [VIM Lazy Load 懒加载／延迟加载技术](https://segmentfault.com/a/1190000017796824)
>> * [搭建基于 Vim 的 C++和 Python 开发环境！ - 知乎](https://zhuanlan.zhihu.com/p/66007069)

### <span id="vimplugin_mgmt_plugpac">Plugpac</span>

[plugpac.vim](https://github.com/bennyyip/plugpac.vim) 是使用 [Vim9脚本](Vimscript9_Note.md) 开发的类似于 [vim-Plug](#vim-Plug) 的插件管理器。

配置示例：

```vim
plugpac#Begin({})

	Pack 'xxx/xxx'

plugpac#End()

```

插件配置在 `plugpac#Begin()` 与 `plugpac#End()` 之间，每个插件使用 `Pack` 来标识，这跟 [vim-Plug](#vim-Plug) 非常类似，使用过 vim-plug 的，迁移到 Plugpac 一点难度都没有。

`plugpac#Begin()` 中还可以进行一些设置，主要就是设置安装、更新插件菜单的样式。如果什么都不配，默认 plugpac 的菜单是横向的，即当前分割成上下两窗口，上面的窗口就是用来显示插件管理。所以还是配下，这样就能跟 [vim-Plug](#vim-Plug) 那竖着的菜单一样了。

```vim
plugpac#Begin({
  status_open: 'vertical',
  verbose: 2,
})
```

---

## <span id="vimplugin_list">插件列表</span>

### <span id="vimplugin_auto_pairs">自动括号匹配</span>

#### auto-pairs

[jiangmiao/auto-pairs](https://github.com/jiangmiao/auto-pairs) 这插件能自动补全匹配的括号。

```vim
Plug 'jiangmiao/auto-pairs'
```

#### vim9 版 auto-pairs

[Eliot00/auto-pairs](https://github.com/Eliot00/auto-pairs) 是 [auto-pairs](#auto-pairs) 的 vim9 适配版。

### <span id="vimplugin_snippets">snippets 插件</span>

#### <span id="vimplugin_snippets_ultisnips">Ultisnips</span>

 #ultisnips

[Ultisnips](https://github.com/SirVer/ultisnips) 是一个 snippet 引擎插件。

```shell
" snippet相关
" snippet调用引擎
Plug 'SirVer/ultisnips'
" snippet仓库
Plug 'honza/vim-snippets'
```

> [!tip]
> Ultisnips 这个插件依赖 python,而且是特定版本，特别恶心,所以慎用！
> 
> 连 [vim-snippets](#plugin_snippets_vimsnippets) 都在文档中表示：「Some people want to use snippets without having to install Vim with Python support. Yes - this sucks.」

##### ultisnips 文档

* [ultisnips的非官方中文文档](https://github.com/Linfee/ultisnips-zh-doc)
* [ultisnips-zh-doc](https://github.com/Linfee/ultisnips-zh-doc/blob/master/doc/UltiSnips_zh.txt)

---

#### <span id="vimplugin_snippets_snipmate">SnipMate</span>

 #snipmate

使用另一个 snippet 引擎：**SnipMate**

**snipmate** 这个插件需要依赖其他两个插件，所需插件如下配置:

```shell
Plug 'MarcWeber/vim-addon-mw-utils'
Plug 'tomtom/tlib_vim'
Plug 'garbas/vim-snipmate'

```

#### <span id="vimplugin_snippets_neosnippet">Neosnippet</span>

[neosnippet](https://github.com/Shougo/neosnippet.vim) 看名字就知道是 Shougo 的作品，这是一个 snippet 引擎，定位跟 [snipmate](#plugin_snippets_snipmate) 或 [ultisnips](#plugin_snippets_ultisnips) 相同。默认使用 [neosnippet-snippets](#plugin_snippets_neosnippet_snippets) 作为 snippet 仓库。

---

#### <span id="vimplugin_snippets_neosnippet_snippets">neosnippet-snippets</span>

 [neosnippet-snippets](https://github.com/Shougo/neosnippet-snippets) 这是为 [Neosnippet](#plugin_snippets_neosnippet) 引擎而设的 snippet 仓库，跟 [vim-snippets](#plugin_snippets_vimsnippets) 类似。

配置：
```vim
" 使用 neosnippet-snippets 作为snippet仓库
let g:neosnippet#snippets_directory = '~/.vim/plugged/neosnippet-snippets/neosnippets'

" 把标记隐藏
if has('conceal')
  set conceallevel=2 concealcursor=niv
endif
```

> [!info] neosnippet 指定 snippets 仓库
> neosnippet 最主要配置就是指定 snippet 仓库的路径。
> 
>neosnippet 除了可以指定默认的 neosnippet-snippets 仓库外，还能指定 [vim-snippets](#plugin_snippets_vimsnippets) 为其 snippets 仓库。

```vim
" 使用Ctrl+k 触发
imap <C-k>     <Plug>(neosnippet_expand_or_jump)
smap <C-k>     <Plug>(neosnippet_expand_or_jump)
xmap <C-k>     <Plug>(neosnippet_expand_target)

smap <expr><TAB> neosnippet#expandable_or_jumpable() ?
\ "\<Plug>(neosnippet_expand_or_jump)" : "\<TAB>"

```

#### <span id="vimplugin_snippets_vimsnippets">vim-snippets</span>

 [vim-snippets](https://github.com/honza/vim-snippets) 是一个 snippet 仓库，它预存储了大量的不同语言的 snippet 文件。为 snippet 引擎，如 [Ultisnips](#Ultisnips) 或 [SnipMate](#SnipMate) 提供「弹药」。

vim-snippets 除了可以使用预制的 snippet 外，还能自定义 snippets 文件。

在~/.vim/目录新建一个目录 **snippets** 目录，用来存在自定义的 snippets 文件。

因为此目录位于所有插件之外，所以你自定义的 snippets 文件不会因为插件更新而被删除。

在 snippets 目录中建立你自定义的 snippets 文件，文件后缀名为 **snippets**。

例如，xml 的 snippets，就新建 **xml.snippets** 文件。

snippets 语法格式请参考 [vim-snippets](https://github.com/honza/vim-snippets)

---

### <span id="vimplugin_format">格式化插件</span>

#### vim-codefmt

此插件是 **google** 开发的!

```shell
Plugin 'google/vim-maktaba'
Plugin 'google/vim-codefmt'
```

vim-maktaba 这个插件得一起装，不然会报错。

vim-codefmt 使用，此插件对于 C、C++、Java 语言，依赖 clang-format，所以得先安装 clang-format,并且设置好默认的格式化配置文件。

各大语言使用的格式化工具:

* C、C++ 和 Java 用的都是 **Clang-Format**
* go 用的是 gofmt
* HTML、CSS、SASS、LESS、JSON 用的是 nodejs 的 **js-beautify**
* rust 用的是 rustfmt

关于 clang-format 请参考：[clang-format](LSP_Complete.md#clang-format)。

vim-codefmt 插件在 vim 中使用，就两个主要命令:

1. **:FormatLines**: 格式化某些行代码

2. **:FormatCode**：格式化整页代码

#### Neoformat

[neoformat](https://github.com/sbdchd/neoformat) 同样也是一款优秀的格式化插件。

这个比 google 的 [vim-codefmt](#vim-codefmt) 好用，支持的编程语言及各编程语言主流的多款格式化器也支持，可以说格式化支持上非常地 [全面](#^neoformat-support-list)，而且配置上也比 vim-codefmt 要简单，而且依赖也没有。

支持列表：[supported-filetypes](https://github.com/sbdchd/neoformat#supported-filetypes) ^neoformat-support-list

安装很简单：

```vim
Plug 'sbdchd/neoformat'
```

可以配置格式化器及为编程语言指定格式化器，示例：

```vim
g:neoformat_python_black = {                                                                                            
   'exe': 'black',
   'args': ['-S'],
   'replace': 1
}
# g:neoformat_enabled_python = ['black', 'autopep8', 'yapf', 'docformatter']
g:neoformat_enabled_python = ['black']

# shell
g:neoformat_sh_shfmt = {
  'exe': 'shfmt',
  'args': ['-i', '0', '-sr', '-ci'],
}

```

> [!tip] 
> 
> 如果像 [Shell](../Linux/Shell_Note.md) 只有一个默认格式化器 [shfmt](../Linux/Shell_Note.md#shfmt)，可以只设置 shfmt，不用再 `enabled` 了。

> [!info] 相关资料
> 
> * [Vim 安装程序 · Prettier 中文网](https://prettier.nodejs.cn/docs/en/vim.html)

###### 格式化命令

* `:Neoformat` 使用默认格式化器来格式化当前文件。
* `:Neoformat! python`：为当前文件指定编程语言进行格式化，使用的是该语言的默认或已配置过的格式化器。
* `:Neoformat! python yapf`：为当前文件指定编程语言及格式化器进行格式化。

### <span id="vimplugin_comment">注释插件</span>

#### <span id="vimplugin_nerdcommenter">nerdcommenter</span>

[nerdcommenter](https://github.com/preservim/nerdcommenter)

简单配置：

```vim
" 注释插入空格
let g:NERDSpaceDelims = 1
" 注释插件空行
let g:NERDCommentEmptyLines = 1

" 自定义 c 语言注释
let g:NERDCustomDelimiters ={'c':{'left':'//'}}

```

nerdcommentor 默认快捷键:

注释： `<Leader>cc`

取消注释：``<Leader>cu`

`<Leader>` 默认为 `\`

> [!tip] nerdcommenter 地址更换
> 注意 此插件原来的 github 的地址为 **scrooloose/nerdcommenter**，如果原来装有的，得改下 Plug 后的字符串值为 **preservim/nerdcommenter**，然后 Clean 下再 Install。

### <span id="vimplugin_statusline">状态栏插件</span>

#### <span id="vimplugin_sl_airline">airline</span>

[vim-airline](https://github.com/vim-airline/vim-airline)

包括 airline 的样式插件

```vim
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'
```

简单配置:

```vim
let g:airline_extensions = ['branch','tabline']

" buffer文件名及路径显示格式
let g:airline#extensions#tabline#formatter = 'unique_tail_improved'

" airline样式设置
let g:airline_theme = 'dark'

" 使用powerline font
let g:airline_powerline_fonts=1

" 设置airline的theme
let g:airline_theme='dark'

```

#### <span id="vimplugin_sl_lightline">lightline</span>

[lightline](https://github.com/itchyny/lightline.vim) 是一个跟 [airline](#airline) 类似的状态栏插件，但它比 airline 更轻量。

##### 配置

linghtline 配色可以单独设置，使用 `g:lightline.colorscheme`：

```vim
" 设置配色
" 判断 g:colors_name 存在不存在
if !exists('g:colors_name')
	let g:lightline.colorscheme = 'wombat'
else
	try
		let g:lightline.colorscheme = g:colors_name
	catch
		let g:lightline.colorscheme = 'wombat'
	endtry
endif

```

> [!info] 
> 
> `let g:lightline = {'colorscheme': 'wombat'}` 只能用于整体设置中。
>

自用配置：

```vim

" 导入通用脚本
import "~/.vim/configs_new/commands/commands_basic.vim"

" 启用标签栏
set showtabline=2

" --------------------------------------------

"  插件配置
" let s:lightline_result = commands_basic#ExistPlug('itchyny/lightline.vim')
let s:lightline_result = s:commands_basic.ExistPlug('itchyny/lightline.vim')
if s:lightline_result ==? 1
	" 检测是否已装 vim-devicons 图标插件	
	let s:devicons_result = s:commands_basic.ExistPlug('ryanoasis/vim-devicons')

	let g:lightline = {
		\ 'active': {
				\ 'left': [ 
					\ [ 'mode', 'paste' ],
					\ [ 'readonly', 'gitbranch', 'filename', 'modified'],
					\ [ 'gitinfo', 'method' ]
				\ ],
				\ 'right': [
					\ [
						\ 'filetype',
						\ 'fileformat',
						\ 'hex', 
						\ 'asc', 
						\ 'lineinfo'
					\ ],
				\ ] 
		\ },
		\ 'separator': {'left': '', 'right': ''},
		\ 'subseparator': {'left': '', 'right': ''},
		\ 'component_function': {
		\ 'filetype': 'MyFiletype',
			\ 'fileformat': 'MyFileformat',
			\ 'gitbranch': 'BranchFugitiveHead',
			\ 'lineinfo': 'LightlineLineinfo',
		\ }
	\ }

	let g:lightline.component = {
		\ 'readonly':  '%{&readonly?"":""}',
	\ }	


	" 检测是否已装 vim-devicons 图标插件	
	" let s:devicons_result = commands_basic#ExistPlug('ryanoasis/vim-devicons')
	if s:devicons_result ==? 1

		" 显示文件类型
		function! MyFiletype() abort
			return winwidth(0) > 70 ? (&filetype !=#'' ? WebDevIconsGetFileTypeSymbol(): 'no ft') : ''	
		endfunction

		" 显示系统及文件编码
		function! MyFileformat() abort
			return winwidth(0) > 70 ? (&fileencoding!=# '' ? &fileencoding.' '.WebDevIconsGetFileFormatSymbol(): &encoding . ' ' . WebDevIconsGetFileFormatSymbol()) : ''. WebDevIconsGetFileFormatSymbol()
		endfunction	


	endif

	" 设置配色
	" 判断 g:colors_name 存在不存在
	if !exists('g:colors_name')
		let g:lightline.colorscheme = 'wombat'
	else
		try
			let g:lightline.colorscheme = g:colors_name
		catch
			let g:lightline.colorscheme = 'wombat'
		endtry
	endif

	" -------------------------------------------------------

	" 显示标签栏
	" let s:llbuffer_result = commands_basic#ExistPlug('mengelbrecht/lightline-bufferline')
	let s:llbuffer_result = s:commands_basic.ExistPlug('mengelbrecht/lightline-bufferline')
	if s:llbuffer_result ==? 1
	
		" let g:lightline                  = {}
		let g:lightline.tabline          = {'left': [['buffers']], 'right': [['close']]}
		let g:lightline.component_expand = {'buffers': 'lightline#bufferline#buffers'}
		let g:lightline.component_type   = {'buffers': 'tabsel'}

		if s:devicons_result ==? 1
			" 启用文件类型图标
			let g:lightline#bufferline#enable_devicons = 1
			" 图标位置 可选left right first 默认left 
			" g:lightline#bufferline#icon_position
		endif
		" 未命名文件名
		let g:lightline#bufferline#unnamed  = '[No Name]'
		" 启用unicode字符 显示"只读" "修改中"等状态
		let g:lightline#bufferline#unicode_symbols = 1

	" --------------------------------------


	endif


endif


" ----------------------------------------



" 行信息
function LightlineLineinfo() abort
	return &filetype ==? 'help'		   ? ''  :
	\ &filetype ==? 'defx'             ? ' ' :
	\ &filetype ==? 'coc-explorer'     ? ' ' :
	\ &filetype ==? 'denite'           ? ' ' :
	\ &filetype ==? 'tagbar'           ? ' ' :
	\ &filetype ==? 'vista_kind'       ? ' ' :
	\ &filetype ==? 'vista'            ? ' ' :
	\ &filetype =~? '\v^mundo(diff)?$' ? ' ' :
	\ printf('%3ld%% %4ld:%3ld', 100*line('.')/line('$'),  line('.'), col('.'))
endfunction

" gitbranch显示样式
function BranchFugitiveHead() abort
	try
		let fh = FugitiveHead()
		return fh !=#''?' '.fh : ''
	catch
		return ''
	endtry
endfunction

```

---

### <span id="vimplugin_cmd_tools">命令行工具</span>

#### autosuggest

[autosuggest](https://github.com/girishji/autosuggest.vim) 是一个命令行自动补全插件。 这插件是使用 [Vim9 脚本](Vimscript9_Note.md) 写的。

![autosuggest screenshot](https://gist.githubusercontent.com/girishji/40e35cd669626212a9691140de4bd6e7/raw/f3e69c22e085f1029a048de422ac37a055e5ac80/autosuggest-demo.gif)

```vim
Plug 'girishji/autosuggest.vim'
```

---

### <span id="vimplugin_filetype_icon">文件类型图标</span>

[vim-devicons](https://github.com/ryanoasis/vim-devicons)

![image-20200618134854822](vim_plugin.assets/image-20200618134854822.png)

### <span id="vimplugin_syntax">语法高亮增强</span>

#### <span id="vimplugin_syn_ployglot">vim-polyglot</span>
[vim-polyglot](https://github.com/sheerun/vim-polyglot)

vim-polyglot 这个插件是插件集，它集成了众多语言相关的插件,语法高亮只是其中一个功能。
用户可以对某子插件进行自行设置。

##### <span id="vimplugin_syn_subplugin">部分子插件</span>

###### markdown

[vim-markdown](https://github.com/plasticboy/vim-markdown)

polyglot 中的 vim-markdown 只有高法及 **Concealing** 功能。
如果想要使用 **vim-markdown** 代码折叠，就得安装 vim-markdown 插件。

vim-markdown 折叠功能相关设置如下:

```vim
let g:vim_markdown_folding_disabled = 1 //0: 开启折叠 1: 关闭折叠
let g:vim_markdown_folding_level = 6 //折叠级别 未设置默认为1

```

其实 vim-markdown 折叠功能有点坑，折叠是折了，但展开输入内容，1 秒就重新折上~
所以还是用 vim8 内置的折叠功能或 [vim-markdown-folding](#vim-markdown-folding) 这个插件好了!

### <span id="vimplugin_operator">符号操作</span>

#### Surround

 #surround

[vim-surround](https://github.com/tpope/vim-surround)

```vim
" ---------------------------------------------------------
"       surround使用
" ---------------------------------------------------------
" 添加双引号 ysiw+"
" 如果要添加tag符号，即尖括号可以使用快捷命令ysiwt
" 注意如果是ysiw<这方式，必须是左尖括号，才是成对tag
" 如果是ysiw> 使用了右尖括号,那就会变成单tag
" 替换格式: cs 符号 符号
" 如将tag符号即<>这种尖括号替换成其他符号可以使用cst快捷命令
" 删除格式: ds 符号
" 行包围符号格式: yss 符号
" 行包围添加小括号快捷命令: yssb
" 行包围尖括号得分两步：yss<或者ysst 然后输入尖括号中的内容
" 行包围尖括号道理同新加一样，分左尖括号和右尖括号，左双右单
"
" ---------------------------------------------------------
```

##### 常用命令

###### normal 模式

* `ds` ：  删除包围
* `cs` ：  修改包围
* `ys` ：  添加包围
* `yS` ：  添加包围并替换包围文本
* `yss` ：  添加一行包围
* `ySs` ：  添加包围内容独成一行
* `ySS` ：  添加包围内容独成一行
* `ysiw"` ：  单词周围加双引号
* `ysiw(` ：  单词周围加圆括号，左括号是带空格的
* `ysiw]` ： 单词周围加方括号，右括号不带空格
* `ysnw` ：  在 **n** 个单词周围加要加的符号或文本
	> [!tip]
    > 如上面的 `ysiw` 类似，`ysiw` 只是 `ysnw` 的特例，是对当前单词加东西
* `ysiWb` ：  以空格为分界加圆括号，这是不带空格的括号，大 `B` 代表不带空格的花括号
* `ysfn` ： 从光标位置到字母 **n** 加 `<span>`
* `ystn"` ： 从光标位置到字母 **n** 加 **"**

###### 可视模式

s  ： 给选中内容添加包围

S  ： 选中内容添加包围并独成一行

###### 插入模式

* `<CTRL-s>` ： 添加一个包围
* `<CTRL-s><CTRL-s>` ： 添加包围内容独成一行
* `<CTRL-g>s` ： 添加一个包围
* `<CTRL-g>S` ： 添加包围内容独成一行

---

### NerdTree

[nerdtree](https://github.com/preservim/nerdtree)

### easymotion

 #easymotion

[vim-easymotion](https://github.com/easymotion/vim-easymotion)

#### easymotion 配置

##### 按键配置

```vim

" 行内跳转
map <Leader><Leader>h <Plug>(easymotion-linebackward)
map <Leader><Leader>l <Plug>(easymotion-lineforward)

" 行间跳转
" easymotion-overwin-line 这是配置多窗口下也能使用行间跳转的快捷键配置
map <Leader><Leader>L <Plug>(easymotion-bd-jk)
nmap <Leader><Leader>L <Plug>(easymotion-overwin-line)

```

如果想使用 `<Leader><Leader>w` 来标记所有词，而不是只标记当光标之下的部分的单词，可以进行以下配置：

```vim
" 所有word跳转
" 为可见区域所有单词都上标记，方便在所有词间跳转
map <Leader><Leader>w <Plug>(easymotion-bd-w)
nmap <Leader><Leader>w <Plug>(easymotion-overwin-w)
```

#### 常用操作

(1)  **跳转到当前光标前后的位置**

`<leader><leader>w`：对当前标一所在位置**往下**所有单词进行标记，以便进行**词间跳转**

`<leader><leader>b`：对当前光标所在位置**往上**所有单词进行标记，以便**词间跳转**

(2) **搜索**

`<leader><leader>s`：搜索包含输入字符的单词并进行标记，以便跳转

(3) **行级跳转**

* `<leader><leader>j`：对当前光标所在位置**往下**的所有**行**进行标记， 以便进行**行间跳转**
* `<leader><leader>k`：对当前光标所在位置**往上**的所有**行**进行标记，以便进行**行间跳转**

(4) **行内跳转**

* `<leader><leader>h`：对当前光标所在行**右边**所有**单词**进行标记，以便进行**行内跳转**
* `<leader><leader>l`：对当前光标所在行**左边**所有**单词**进行标记，以便进行**行内跳转**

#### 相关资料

* [vim插件: easymotion快速跳转](https://wklken.me/posts/2015/06/07/vim-plugin-easymotion.html)

### vim9-stargate

[vim9-stargate](https://github.com/monkoose/vim9-stargate) 这个是 [easymotion](#easymotion) 的 vim9 适配版。它使用的 vim9 的语法重写了部分功能，相比原版的 [easymotion](#easymotion)，功能还是有点弱。

### undo tree

[undotree](https://github.com/mbbill/undotree)

```vim
" -----------------------------
"       UndoTree设置
" -----------------------------
"映射快捷键
nnoremap <leader>udt :UndotreeToggle <CR>
" 设置undo文件的存放目录(得事先mkdir)
set undofile
set undodir=~/.local/share/nvim/.undodir
```

### vim-gitgutter

[vim-gitgutter](https://github.com/airblade/vim-gitgutter)

基础配置

```vim
" -----------------------
"       GitGutter设置
" -----------------------
" 开启gitgutter
let g:gitgutter_enabled = 1

```

vim-gigutter 各种常用命令:

 `:GitGutterToggle`： 开启关闭 gutter

 `:GitGutterLineHighlightsToggle`： 开启关闭高亮相关行

### vim-fugitive

[vim-fugitive](https://github.com/tpope/vim-fugitive)

**常用命令:**

```shell
:Git commit
:Git push
```

**:Git** 后加 git 的常用命令，跟在终端下使用 git 相同，push 提交时会切出终端输入远程仓库的用户名和密码。

如果在 neovim 中使用**:Git push**不能弹出输入用户名和密码，就使用**:terminal git push**

### LoremIpsum

[loremipsum](https://github.com/vim-scripts/loremipsum)

常用命令:

```shell
:Loremipsum  " 生成默认文本
:Loremipsum 数字 " 生成指定字符数目的文本
```

### <span id="vimplugin_preview">预览插件</span>

[vim-livedown](https://github.com/shime/vim-livedown)

此插件使用到 nodejs 模块 livedown，所以得先安装 node 的模块:

```shell
npm install -g livedown
```

.vimrc 中加入以下代码:

```shell
Plug 'shime/vim-livedown',{'on':['LivedownPreview','LivedownToggle','LivedownKill']}
```

后面那 {'on':....} 是 Plug 的 **按需加载** 的设置

使用 python 的模块 [grip](https://github.com/joeyespo/grip)，也能预览 markdown

首先安装安装 grip

```shell
  pip install grip
```

---

### <span id="vimplugin_tools">小工具</span>

#### <span id="vimplugin_tools_startuptime">Startuptime</span>

[Startuptime](https://github.com/dstein64/vim-startuptime)

![startuptime screenshot](https://github.com/dstein64/media/blob/main/vim-startuptime/screenshot.png?raw=true)

这是个一个测试 vim 各组件、插件占用启动时长的小工具。

---

#### vim-startify

[vim-startify](https://github.com/mhinz/vim-startify) 是一个 vim/neovim 启动页面的插件。

 可以手动配启动页面显示的图案
```vimscript


let g:startify_custom_header =[
		\ '  _   ___      _______ __  __  ',
		\ ' | \ | \ \    / /_   _|  \/  | ',
		\ ' |  \| |\ \  / /  | | | \  / | ',
		\ ' | . ` | \ \/ /   | | | |\/| | ',
		\ ' | |\  |  \  /   _| |_| |  | | ',
		\ ' |_| \_|   \/   |_____|_|  |_| ',
	\ ]

```

也可以使用系统的 [figlet](http://www.figlet.org) 工具生成。

安装 figlet：
```shell
pacman -S figlet
```
> [!info] 类似工具
> 与 figlet 类似的小工具，还有 `banner`、 `toilet`，哈哈，这名字有点脏，用法大同小异。

figlet 常用参数：
* `-f`：指定字体样式
* `-w`：指定输出的宽度

大概用法：
```shell
# 最简单的用法
figlet 要输出的文字
# 指定个字体
figlet -f xx 要输出的文字
# 指定输出的宽度
figlet -w 数字 要输出的文字
```

在 startify 的配置中可以调用 figlet 来生成 vim 启动页面那个图案。

直接贴出 github 上的示例：

```vimscript
let g:startify_custom_header =
       \ startify#pad(split(system('figlet -w 100 VIM2020'), '\n'))
```

字体 [GitHub](../Git/Git_Note.md#git_github) 库：[figlet-fonts](https://github.com/xero/figlet-fonts)

```shell
██╗  ██╗███████╗██╗     ██╗      ██████╗ 
██║  ██║██╔════╝██║     ██║     ██╔═══██╗
███████║█████╗  ██║     ██║     ██║   ██║
██╔══██║██╔══╝  ██║     ██║     ██║   ██║
██║  ██║███████╗███████╗███████╗╚██████╔╝
```
> [!tip] 
> 
> 这种字体是：`ANSI Shadow`。
>
> 使用 `-f` 指定字体时，如果字体名称有空格，应用引号 `'` 括起来。

---

#### <span id="vimplugin_mcursors">多光标</span>

##### <span id="vimplugin_mcursors_1">vim-visual-multi</span>

[vim-visual-multi](https://github.com/mg979/vim-visual-multi) 这个插件能使 vim 进行多光标操作。

常用操作步骤：

`Ctrl-n`：进入多光标模式，并选中当前光标所在字符。

在启动多光标模式的，继续按 `n`，能够选中相同的下一个字符。

选择完成，可以按 `i`，`a`，`I`，`A` 进入 [Insert 模式](Vim_Note.md#vim_mode_insert) ，继续以下的操作。

按 <kbd>Exit</kbd> 可退出多光标模式。

---

#### <span id="vimplugin_hightlight">高亮</span>

##### <span id="vimplugin_hlyank">高亮复制</span> 

[vim-highlightedyank](https://github.com/machakann/vim-highlightedyank) 是一个实现了在复制操作时，高亮一下刚复制的文本功能的插件。

安装：
```vim
	" vim 8.0+
	Plug 'machakann/vim-highlightedyank'

	" 如果是vim 8.0以前的版本
	if !exists('##TextYankPost')
		map y <Plug>(highlightedyank)
	endif
```

高亮持续时间：
```vim
	" 默认是亮了一秒
	" 单位是毫秒
	let g:highlightedyank_highlight_duration = 1000
```

还能设置高亮的颜色：
```vim
	highlight HighlightedyankRegion cterm=reverse gui=reverse
```
> [!tip]
> 高亮颜色设置要放在 colortheme 设置之后。

#### <span id="vimplugin_mark">mark 相关</span>

##### <span id="vimplugin_mk_signature">signature</span>

[vim-signature](https://github.com/kshenoy/vim-signature) 是一个 mark 显示插件。
在侧边栏显示 mark 标记。

![vim_signature shotcut](vim_plugin.assets/vim_plugin_signature.png)

常用操作：

| 命令 | 说明 |
| :---: | :---: |
| mx | 添加 mark x 是该 mark 的名称 可以是大小写字母 |
| dmx | 移除当前行某个 mark x 是添加时的名称 |
| m- | 移除当前行所有 mark |

### vimspector

[vimspector](https://github.com/puremourning/vimspector)

这是一个 vim 下多语言图形界面 debug 插件!
> [!quote]
> A multi language graphical debugger for Vim

安装:

```vim
Plugin 'puremourning/vimspector'
```

---

#### <span id="vimplugin_browser">浏览器插件</span>

##### <span id="vimplugin_browser_openbrowser">open-browser</span>
[open-browser](https://github.com/tyru/open-browser.vim)

打开浏览器并跳转到指定网址。

全部命令如下图：

![open-browser_1](./vim_plugin.assets/vim_plugin_open-browser_1.png)

![open-browser_2](./vim_plugin.assets/vim_plugin_open-browser2.png)

##### <span id="vimplugin_browser_github">open-browser-github</span>

快速打开 github。

[browser_github](https://github.com/tyru/open-browser-github.vim)

全部命令如下图：

![](./vim_plugin.assets/vim_plugin_open-github.png)

---

### <span id="vimplugin_markdown">Markdown 相关插件</span>

 #markdown 

#### [vim-markdown-folding](https://github.com/masukomi/vim-markdown-folding)

```vim
 let g:markdown_fold_style = 'nested' // 默认不设置是Stacked
 set foldlevel=3   //折叠级别

```

还有其他命令:

* `:set foldlevel=数字`： 设置折叠级别
* `zM`： 相当于 set foldlevel=0

#### <span id="vimplugin_md_privew_1">Markdown 预览插件</span>

Markdown 预览插件原理大同小异，都是通过启动一个小型［服务器］来加载渲染 Markdown 页面，从而实现预览效果。
这小型［服务器］有可能是用 Python 实现，也有可能是 NodeJS 或其他技术。

[markdown-preview](https://github.com/iamcco/markdown-preview.nvim)

这个插件是 NodeJS 实现，所以系统得装有 NodeJS 并且装上 Yarn。
常用命令：

```vim
 Start the preview
:MarkdownPreview

" Stop the preview"
:MarkdownPreviewStop
```

##### <span id="vimplugin_md_privew_2">vim-markdown-preview</span>

[vim-markdown-preview](https://github.com/JamshedVesuna/vim-markdown-preview)
这个插件是通过 Python 实现的,要使用此插件得先装 [Grip](#https://github.com/joeyespo/grip)(--GitHub Readme Instant Preview)

##### <span id="vimplugin_md_privew_3">preview-markdown.vim</span>

[preview-markdown.vim](https://github.com/skanehira/preview-markdown.vim)

此插件需要 [mrd](https://github.com/MichaelMure/mdr)(--MarkDown Renderer)
>mdr is a standalone Markdown renderer for the terminal.
因为这个插件是在 vim 内部使用 terminal 方式预览，所以对 vim 版本有限制：Vim 8.1.1401+

#### <span id="vimplugin_md_table">Markdown 表格相关的插件</span>

##### <span id="vimplugin_md_table_1">VIM Table Mode</span>

[VIM Table Mode](https://github.com/dhruvasagar/vim-table-mode) 这个插件能够简化绘制表格操作。

使用 **:TableModeToggle** 命令 或 `<Leader>tm` 快捷命令启动 Table 模式。
使用 **:TableModeDisable** 命令就能退出 Table 模式。

使用小技巧:

markdown 表格的对齐方式那个行的输入。
如果是居中对齐, markdown 表格要求是 **| :---: |**这种格式 ，<br> 
在此插件下，可以 输入
```markdown
|:-:|
```
就能快速 " 生成 " 一个 **| :---: |** 这个对齐项

##### <span id="vimplugin_md_table_2">markdowntable</span>

[markdowntable](https://github.com/nora75/markdowntable) 用来快速生成一个表格。

常用操作:
```vim
:TableMake 行数 列数
```

---

## 相关笔记

* [Vim 笔记](Vim_Note.md)
* [vim常用操作](vim常用操作.md)
* [Vim 视频清单](Vim_Videos.md)
* [vim及neovim配置](vim及neovim配置.md)
* [vimscript 笔记](Vimscript_Note.md)
* [vim 配色笔记](vim_colorscheme_Note.md)

### <span id="lsp_complete">关于 LSP 及补全</span>

[LSP及补全](./LSP_Complete.md)
