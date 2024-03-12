---
aliases: []
tags:
  - vim
  - lsp
created: 2023-08-18 19:44:52
modified: 2024-03-11 20:15:26
---

# LSP 及补全相关

---

## 目录

* [关于LSP](#about_lsp)

* [Vim LSP Client插件](#vp_lsp_client)
	* [vim-lsc](#vp_lsp_client_vim-lsc)
	* [vim-lsp](#vp_lsp_client_vim-lsp)
	* [LanguageClient-neovim](#vp_lsp_client_lcn)
	* [Yegappan](#vp_lsp_client_yegappan)

* [Vim 补全插件](#vp_complete)
	* [vim-auto-popmenu](#vp_complete_vim_auto_popmenu)
	* [neocomplete](#vp_complete_neocomplete)
	* [deoplete](#vp_complete_deoplete)
		* [使用vim-lsc为LSC](#vp_deoplete_lsc)
		* [使用vim-lsp为LSC](#vp_deoplete_lsp)
		* [使用LanguageClient](#vp_deoplete_lcn)
	* [Completor](#vp_complete_completor)
	* [ncm/ncm2](#vp_complete_ncm)
	* [asyncomplete](#vp_complete_asyncomplete)
	* [coc](#vp_complete_coc)
	* [easycomplete](#vp_complete_easycomplete)

---

## <span id="about_lsp">关于 LSP</span>

> [!quote] 官方定义
> The Language Server Protocol (LSP) defines the protocol used between an editor or IDE and a language server that provides language features like auto complete, go to definition, find all references etc.

一种用于为编辑器或 IDE 提供，诸如自动补全、定义跳转、查找关联等语言功能的编程语言服务协议。

LSP 相关网站:
* [LSP规范](https://microsoft.github.io/language-server-protocol/specifications/specification-current/) 
* [LSP官网](https://microsoft.github.io/language-server-protocol/) [![LSP Repo](https://img.shields.io/github/stars/microsoft/language-server-protocol?style=social)](https://github.com/microsoft/language-server-protocol)
* [各家LSP实现列表](https://microsoft.github.io/language-server-protocol/implementors/servers/)

---

## <span id="lang_lsps">常用语言 LSP</span>

从 LSP 官网给的 [列表](https://microsoft.github.io/language-server-protocol/implementors/servers/)，能看到各种语言的 LSP 实现。 

### <span id="lang_lsps_ccpp">C/C++</span>

clangd
clangd 是 clang 的扩展工具（clang-tools-extra），新版本是在 LLVM 中,安装 LLVM 就有 clangd 了。

安装完可以执行以下命令，如果能出现版本信息就证明 clangd 能用了！

```shell
clangd --version
```

#### clang-format

clang-format 是 clangd 附带的格式化工具。

clang-format 全局配置文件是放在用户根目录下的**.clang-format**,既~/.clang-format。

clang-format 配置文件不必全手工，可以使用 clang-format 生成指定风格的模板文件，然后再在此文件上进行自定义修改即可。

```shell
clang-format -style=格式名 -dump-config > 文件名
```
> [!info] 
> 
> clang 的格式风格有很多，如 google、微软、Mozilla 等。
> 
> 文件名可以取任何名字，一般取 `.clang-format` 或 `_clang-format`，这样能让 clang-format 识别。

> [!info] clang-format 格式文档
>
> [Clang-Format Style Options ](https://clang.llvm.org/docs/ClangFormatStyleOptions.html)

生成 llvm 风格的模板：

`clang-format -style=llvm -dump-config > .clang-format`

生成 google 风格的模板：

`clang-format -style=google -dump-config > .clang-format`

##### clang-format 配置

###### 示例
 
```yaml
BasedOnStyle: Google     # 配置格式化基于哪家的风格 有Google LLVM 微软等
# BasedOnStyle: LLVM
# BasedOnStyle: Microsoft
IndentWidth: 4       # 缩进宽度
TabWidth: 4        # tab缩进宽度
# UseTab: Always
UseTab: AlignWithSpaces     # 是否使用Tab缩进
AllowShortFunctionsOnASingleLine: Empty # 简单函数格式化成单行 Empty是函数体是空的才格式化成单行样式
AllowShortBlocksOnASingleLine: Empty # 简单代码块格式化成单行 Empty是代码块是空的才格式化单行样式
AlignConsecutiveAssignments: true  # 连续赋值对齐
# AlignConsecutiveDeclarations: true # 连续声明对齐
```

> [!infot] 配置详解
> 
> 使用 `clang-format -style=格式名 -dump-config > 文件名` 这个语法生成的配置文件，默认会将 `BaseOnStyle` 注释掉，而把全部属性都在文件中显式地设置一遍 -- 其实就是将某厂商风格配置文件 Copy 一份过来而已，这非常不「优雅」。
> 
> 而更好的做法应该是，将 `BaseOnStyle` 属性注释取消，这意味着明确告诉 clang-format 格式化器，配置文件是使用哪家厂商的风格为底板，配置文件中设置的属性是自定义设置的外，其他属性都使用 `BaseOnStyle` 指定的厂商默认配置。这样设置，就使用配置文件中的配置项非常的少，以一种更「优雅」的方式自定义自己的格式化风格。

---

### <span id="lang_lsps_python">Python LSP</span>

python lsp 实现：

~~[微软的 python lsp](https://github.com/Microsoft/python-language-server) ：VSCode 中 python 提示、语法分析等功能，用的是就是这个 LSP。VSCode 中 [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) [![Python extension for VSCode Repo](https://img.shields.io/github/stars/Microsoft/vscode-python?style=social)](https://github.com/Microsoft/vscode-python) 插件其实就是这个 LSP 的 Client。这个 LSP 一般是安装 VSCode Python 插件时，一起安装的。~~

[Pydev on VSCode ](https://www.pydev.org/vscode/index.html)： 这是 Pydev 针对 VSCode 的 python 插件。这个插件应该是既是 Server 也是 Client。

之前两种 LSP 跟 插件耦合太明显，下面几款 LSP 能在各种编辑器用：

---

下面几款 Python LSP 名字都有点相近：

#### pyright

[pyright](https://github.com/microsoft/pyright) 是微软新推出的 LSP，上面那个要被微软废弃了！因为微软 [VSCode](../Editors/VSCode_Note.md) 自己用的是 Pylance，所以开源的 Pyright 估计会被微软阉割，所以下场也不会太好！

##### 安装

pyright 可以用 npm 装也可以用 [pip](../Python/Python_Note.md#pip) 装，不过还是建议使用 npm 装，毕竟人家是用 [TypeScript](../JS/TypeScript/TypeScript_Note.md) 写的嘛！

```shell
npm i -g pyright
```

#### jedi 

[jedi](https://github.com/davidhalter/jedi) 这是 Python 界大名鼎鼎的 LSP，很多后来的 Python LSP 都是它的子孙。

#### python-language-server

[python-language-server](https://github.com/palantir/python-language-server) 这款 LSP 是基于 [jedi](#jedi) 。 这款 LSP 需要 Python 版本是 **3.5+**。 这款 LSP 是 [vim-lsp](https://github.com/prabirshrestha/vim-lsp) 示例配置中 Python 的 LSP。 这款 LSP 依赖的 [jedi] 版本相对「保守」点。

python-language-server 安装：

```shell
pip install 'python-language-server[yapf]'
# 或者
pip install 'python-language-server[all]'
```
如果报 `'install_requires' must be a string or list of strings` 类似错误，请执行以下代码：
```shell
pip install -U setuptools
```

测试 python-language-server 是否安装成功：`pyls --help`。

这个 LSP 已经是「**unmaintained**」,所以建议别用了！

#### jedi-language-server

[jedi-language-server](https://github.com/pappasam/jedi-language-server) 灵感源于上面那款。 这款 LSP 要求 Python 的版本是 **3.7+**。 这款 LSP 是比较新的，在 vim 下 使用效果挺不错的。~~这也是几个 python 版本 lsp 在 [deoplete](#deoplete)+[vim-lsp](#vim-lsp)，能正常代码提示的 LSP。~~

> [!tip] 
> 
> 其他 python 的 LSP 代码提示不正常，有可能是依赖的 jedi 等模块版本发生冲突，这大概是直接在 pip 中安装多个 python 的 LSP 所致。所以建议，如果想换着 LSP 来「玩」，可以使用 [pipx](../Python/Python_Note.md#pipx) 来装，这样每个 LSP 都有各自的虚拟环境，互不干扰。

jedi-language-server 安装：

```shell
pip install -U jedi-language-server
```

#### python-langserver

~~[python-langserver](https://github.com/sourcegraph/python-langserver) 这个 LSP 已经停止维护了！~~

#### python-lsp-server

[python-lsp-server](https://github.com/python-lsp/python-lsp-server) 同样是一款基于 [jedi](#jedi)，由 Spyder IDE 的团队在维护的 Python LSP。Spyder IDE 用的也是这个 LSP 实现。这个 LSP 是要求 Python 的版本是  **3.7+** 。

python-lsp-server 安装：

```shell
pip install "python-lsp-server[yapf]"
# 或者
pip install "python-lsp-server[all]"
```

安装出现 `'install_requires' must be a string or list of strings` 类似的错误，请执行以下代码：

```shell
pip install -U setuptools
```

测试是否安装成功：`pylsp -V`。

这个 Python LSP 应该是当下主流使用的，毕竟至今还在更新。

> [!tip] 注意
> 
> pylsp 这个 LSP 用的 [jedi](https://github.com/davidhalter/jedi) 模块的版本可能与 [jedi-language-server](#jedi-language-server) 存在差异，如果使用 [pip](../Python/Python_Note.md#pip) 直接安装，可能造成不必要的冲突，建议使用 [pipx](../Python/Python_Note.md#pipx) 来装，这样两个 LSP 都在各自的虚拟环境中运行，互不干扰。

#### <span id="lang_lsps_python_ruff">ruff</span>

[ruff](https://github.com/astral-sh/ruff)

##### ruff-lsp

[ruff-lsp](https://github.com/astral-sh/ruff-lsp)

---

### <span id="lang_lsps_lua">Lua LSP</span>

#### <span id="lang_lsps_lua_lualsp">lua-lsp</span>

[lua-lsp](https://github.com/Alloyed/lua-lsp) 这个 LSP 只支持 lua 版本到 5.3。

#### <span id="lang_lsps_lua_lua-language-server">lua-language-server</span>

[lua-language-server](https://github.com/LuaLS/lua-language-server)

---

### <span id="lang_lsps_ruby">Ruby LSP</span>

#### <span id="lang_lsps_ruby_rubylanguageserver">ruby_language_server</span>

[ruby_language_server](https://github.com/kwerle/ruby_language_server)

#### <span id="lang_lsps_ruby_rubylsp">Ruby LSP</span>

[ruby-lsp](https://github.com/Shopify/ruby-lsp)

安装：

```shell
gem install ruby-lsp
```

#### <span id="lang_lsps_ruby_solargraph">Solargraph</span>

[Solargraph: A Ruby language server.](https://github.com/castwide/solargraph)

安装：

```shell
gem install solargraph
```

#### <span id="lang_lsps_ruby_sorbet">sorbet</span>

[sorbet](https://github.com/sorbet/sorbet) 安装完后， 使用 `srb` 来调用。

```shell
gem install sorbet
gem install sorbet-runtime
```

#### <span id="lang_lsps_ruby_steep">steep</span>

[steep](https://github.com/soutaro/steep)

```shell
gem install steep
```

#### 相关资料

* [Solargraph vs Ruby LSP: which one to choose?](https://achris.me/posts/solargraph-vs-ruby-lsp/)

---

### <span id="lang_lsps_yaml">YAML LSP</span>

#### <span id="lang_lsps_yaml_yaml-language-server">yaml-language-server</span>

[yaml-language-server](https://github.com/redhat-developer/yaml-language-server) 这个是 RedHat 提供的 yaml 的 LSP。

安装：

```shell
npm install yaml-language-server -g
```

### <span id="lang_lsps_rust">Rust LSP</span>

请参考笔记：[Rust LSP](../Rust/Rust_Note.md#rust_lsp)

---

### <span id="lang_lsps_vim">Vim LSP</span>

#### <span id="lang_lsps_vim_vim-language-server">vim language server</span>

[vim language server](https://github.com/iamcco/vim-language-server)

安装：

```shell
npm install -g vim-language-server
```

最好连 `vscode-langservers-extracted` 这个也装上：

```shell
npm i -g vscode-langservers-extracted
```

有的 LSP 会用到这：[nvim-lspconfig server configuration docs about html](https://github.com/neovim/nvim-lspconfig/blob/master/doc/server_configurations.md#html)

---

### <span id="lang_lsps_html">HTML LSP</span>

html 的 LSP 就没什么可选的，因为这东西编辑器本身就能实现代码提示，所以 LSP 大概就只有微软的：[html-language-server](https://github.com/microsoft/vscode/blob/main/extensions/html-language-features/)。

安装：

```shell
npm install -g vscode-html-languageserver-bin
```

这个 LSP 运行程序名是：`vscode-html-languageservice`。

---

### <span id="lang_lsps_css">CSS LSP</span>

#### <span id="lang_lsps_css_css-language-server">css-language-server</span>

[css-language-server](https://github.com/Microsoft/vscode/tree/main/extensions/css-language-features/server)

安装：

```shell
npm install css-language-server -g
```

css-language-server 的程序名：`css-languageserver`

当然，也可以安装另一个的 css lsp：

```shell
npm install -g vscode-css-languageserver-bin
```

> [!tip]
> 
> 还是建议装 `vscode-css-languageserver-bin` 这个 lsp，上面那个 lsp，版本号才 0.0.2，看那个鬼样子，基本不怎么更了。

---

### <span id="lang_lsps_typescript">TypeScript LSP</span>

#### <span id="lang_lsps_typescript_tls">typescript-language-server</span>

~~[typescript-language-server](https://github.com/prabirshrestha/typescript-language-server)~~ 这个 LSP 已过时。

#### <span id="lang_lsps_typescript_tsserver">Typescript language server</span>

名字完全一样。这个 [typescript-language-server](https://github.com/typescript-language-server/typescript-language-server) 是上面那款的「继任者」。

这个 LSP 程序名：`tsserver`

安装：

```shell
npm install -g typescript-language-server typescript
```

> [!info]
> 
> 如果 `typescript` 已经安装，可以省略：`npm install -g typescript-language-server`

运行：

```shell
typescript-language-server --stdio
```

---

### <span id="lang_lsps_vue">Vue LSP</span>
	
[VLS](https://www.npmjs.com/package/vls) [![VLS Repo](https://img.shields.io/github/stars/vuejs/vetur?style=social)](https://github.com/vuejs/vetur/tree/master/server)

安装：

```shell
npm install vls -g
```

---

### <span id="lang_lsps_other">其他 LSP</span>

#### bash-language-server

[bash-language-server](https://github.com/bash-lsp/bash-language-server) 顾名思义这是一款 [Bash](../Linux/Shell_Note.md#Bash) 的 LSP。

使用 [nodejs](../Node/NodeJS_Note.md) 安装：

```shell
npm i -g bash-language-server
```

> [!info] 相关资料
> 
> * [bash-language-server vim 配置](https://github.com/bash-lsp/bash-language-server#vim)

#### diagnostic-languageserver

[diagnostic-languageserver](https://github.com/iamcco/diagnostic-languageserver) 这个是一个通用诊断语言服务器。

主要有两功能：

* 诊断
* 格式化

这个 LSP 对于那些没有专门的 LSP 的编程语言，如 [Shell](../Linux/Shell_Note.md)，是非常好的补充，至少有个 LSP 可用的。

```shell
npm install diagnostic-languageserver -g
```

---

## <span id="vp_lsp_client">vim LSP Client 插件</span>

LSP Language Server Protocol 为语言提供语言服务，有 Server 肯定就要有 Client。

vim 也需要一个 Client 去与 LSP「对接」。这就是 LSC--Language Server Client。

vim 本身没有提供 LSC(据说未来版本会逐步增加这块),所以得通过插件来实现。

LSC 只是提供与 LSP 对接，并将 LSP 传来的语言服务获取补全数据。
而补全数据需要「展示」出来，如果不装补全插件，那这些数据是传给 vim，使用 vim 本身的补全来将数据「展示」。

常用 LSC 插件

### <span id="vp_lsp_client_vim-lsc">vim-lsc</span>

[vim-lsc](https://github.com/natebosch/vim-lsc)

```vim
	" 开启lsc	
	let g:lsc_enable_autocomplete  = v:true
	" 
	set completeopt=menu,menuone,noinsert,noselect

```

配置 LSP,为各语言指定使用 LSP。
如下示例:c 和 c++ 用的是 clangd，python 用的是 pyls(python-language-server)。

```vim
	let g:lsc_server_commands = {
	 \ 'c': {},
	 \ 'cpp':{
	 \	'command':'clangd --background-index',
	 \  'suppress_stderr': v:true
	 \ },
	 \ 'python':{
	 \  'command':'pyls'
	 \ },
	 \ rust':{
     \  'command':'rls'
     \ }
	 \}	

```
**command**指定是 LSP 名称,就是在终端中能调出 LSP 那个名称。

---

### <span id="vp_lsp_client_vim-lsp">vim-lsp</span>

[vim-lsp](https://github.com/prabirshrestha/vim-lsp)

```vim
	
	" 关闭lsp的语法诊断
	let g:lsp_diagnostics_enabled = 0

	" 设置各语言LSP
	if executable('clangd')
		au User lsp_setup call lsp#register_server({
			\ 'name': 'clangd',
			\ 'cmd': {server_info->['clangd', '-background-index']},
			\ 'whitelist': ['c', 'cpp', 'objc', 'objcpp'],
		\ })
	endif
	
	if (executable('pyls'))
		au User lsp_setup call lsp#register_server({
		\ 'name': 'lsp-pyls',
		\ 'cmd': {server_info->['pyls']},
		\ 'allowlist': ['python']
		\ })
	endif

```

[SpaceVim](vim及neovim配置.md#SpaceVim) 默认用的就是 vim-lsp。

vim-lsp 的一堆命令，都是以 `Lsp` 打头的：

* `LspStatus`：这个非常常用，可以查看 LSP 运行状态，看配置是否成功。

#### <span id="vp_vim-lsp_vim-lsp-neosnippet">vim-lsp-neosnippet</span>

[vim-lsp-neosnippet](https://github.com/thomasfaingnaert/vim-lsp-neosnippet) 是一个将 [vim-lsp](#vim-lsp) 整合 [Neosnippet](vim_plugin.md#plugin_snippets_neosnippet) 的插件。

#### <span id="vp_vim-lsp-ultisnips">vim-lsp-ultisnips</span>

[vim-lsp-ultisnips](https://github.com/thomasfaingnaert/vim-lsp-ultisnips) 是一个将 [vim-lsp](#vim-lsp) 整合 [Ultisnips](vim_plugin.md#Ultisnips) 的插件。

---

### <span id="vp_lsp_client_lcn">LanguageClient-neovim</span>

[LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim) 是用 Rust 语言写的一个 LSC 插件。

这个 LSC 可以为 [deoplete](#vp_complete_deoplete) 及 [ncm2](#vp_complete_ncm) 补全框架提供补全数据源。

LanguageClient 为补全框架提供源的名称是 `LanguageClient`。

安装：

```Vim
	Plug 'autozimu/LanguageClient-neovim', {
	\ 'branch': 'next',
	\ 'do': 'bash install.sh',
	\ }
	
```

配置：

```vim
	" 为各语言指定LSP	
	let g:LanguageClient_serverCommands = {
	\ 'c':['clangd'],
	\ 'cpp':['clangd'],
	\ 'rust': ['rls'],
	\ 'python': ['pyls'],
	\ 'ruby': ['solargraph', 'stdio'],
	\ }


```

---

### <span id="vp_lsp_client_yegappan">Yegappan</span>

[GitHub - yegappan](https://github.com/yegappan/lsp) 这个 LSC 只支持 vim 9.0 。

---

## <span id="vp_complete">Vim 补全插件</span>

### <span id="vp_complete_vim_auto_popmenu">vim-auto-popmenu</span>

[vim-auto-popmenu](https://github.com/skywind3000/vim-auto-popmenu) 这个插件实现了基本的补全功能。

这个插件用到的数据源来自 buffer, dict, tags。可以说这个插件是为了那些不需要装 [LSP](#about_lsp)，又想有基础的补全功能的场景使用而出的极轻量补全插件。

这插件作者还非常「贴心」地另外制作了一个词典插件：[GitHub - skywind3000/vim-dict: 没办法，被逼的，重新整理一个词典补全的数据库](https://github.com/skywind3000/vim-dict)，以弥补补全源数据过于「朴素」。这词典包括了 c、c++、java、python、go、javascript 等语言常用的词汇。

这个插件，在选定候选项，回车确认，默认情况会发生不但确认了而且自动换行的行为；这明显不是大部分人所需要的（这种需求，估计是写 [Python](../Python/Python_Note.md) 的），所以为了禁止确认后自动换行的行为发生，可以在设置里添加以下代码：
```vim
" 1：确认不换行
" 0：确认并换行
let g:apc_cr_confirm = 1
```
> [!quote] 换行的 issue
> [let \<CR\> confirm select other than create new line · Issue #4 · skywind3000/vim-auto-popmenu · GitHub](https://github.com/skywind3000/vim-auto-popmenu/issues/4) 

---

### <span id="vp_complete_neocomplete">neocomplete.vim</span> 

[neocomplete](https://github.com/Shougo/neocomplete.vim)

neocomplete 不兼容 vim8.2。而已没再来更新新功能，只有修 bug。

这插件必须是 vim7.3.855 以上 vim8 以下的版本，而且是拥有 lua 特性的版本使用。

这个插件现在基本可以忽略。

---

### <span id="vp_complete_deoplete">deoplete</span>

[deoplete](https://github.com/Shougo/deoplete.nvim) 是 [neocomplete](#vp_complete_neocomplete) 的改进版，适配 vim8+ 及 neovim。deoplete 内置了路径补全。

虽然叫补全框架，但实际框架需要与 Language Server Client 插件通信，拿到补全数据，才能将数据展示出来。
所以这就涉及到也 LSC 插件的配置。有的补全框架，自己给了部分语言的 LSC 实现，有的是通过支持第三方 LSC 插件来实现。deoplete 既有自己的 LSC，也支持多种 LSC 插件。

#### <span id="vp_complete_deoplete_install">deoplete 安装</span>

deoplete 安装有两个前置条件:

1. vim8 或者 neovim 而且是拥有 python3 特性
  在 vim 中用以下命令检测当前 vim 是否拥有 python3 特性
  ```vim
		:echo has("python3")
  ```
2. pynvim
```shell
pip3 install pynvim
```
> [!tip] pynvim
> 如果没装 pynvim，会引发 `pythonx import [pynvim|neovim]command to work` 的错误提示。

如果以上两个条件满足，就可以安装 deoplete 插件：
```vim
	if has('nvim')
	  Plug 'Shougo/deoplete.nvim', { 'do': ':UpdateRemotePlugins' }
	else
	  Plug 'Shougo/deoplete.nvim'
	  Plug 'roxma/nvim-yarp'
	  Plug 'roxma/vim-hug-neovim-rpc'
	endif
```

deoplete 配置:
```vim
	" 启动deoplete
	let g:deoplete#enable_at_startup = 1
	" 补全延迟，默认是20毫秒
	let g:auto_complete_delay=10


```

deoplete 快捷捷映射配置:
```vim
	" 补全菜单选择映射为用Tab键(默认是Ctrl-n和Ctrl-p)
	inoremap <expr> <Tab> pumvisible() ? "\<C-n>" : "\<Tab>"
	inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>"
	inoremap <expr> <cr> pumvisible() ? "\<C-y>" : "\<cr>"	

```

最关键一步到了，就是配置补全源。
补全源，大体有两个，一个来自 snippet，另一个就是来自 LSC 接口/插件的。

snippet 主流在两个 [snipmate](https://github.com/garbas/vim-snipmate) 和 [ultisnips](http://github.com/SirVer/ultisnips)。 当然 deoplete 自己也有一个 snippet 引擎：[neosnippet](https://github.com/Shougo/neosnippet.vim)。无论哪个 snippet 引擎，其 snippet 「仓库」大概都倾向使用 [vim-snippets](https://github.com/honza/vim-snippets)。

deoplete 与 snipmate 整合，可以使用 [deoplete-snipmate](https://github.com/dcampos/deoplete-snipmate) 这个插件作为连接插件。

deoplete 与 ultisnips 整合时，需要注意的，是 ultisnip 的 expand 代码的快捷键跟 deoplete 选择候选项的快捷键一样为 `Tab` 键，这样就会产生冲突，使得 候选项虽然列出了，但没法使用 `Tab` 键选择，所以最好的方案就是将 ultisnips 的 expand 代码的快捷键设成其他，这样侯选项就能选了。如以下示例，就将 ultisnips expand 代码的快捷键设为 `Ctrl+e`：
```vimscript
let g:UltiSnipsExpandTrigger = "<c-e>"
```

Shougo 大神为 deoplete 提供了一些语言的 LSC，比如 c/c++ 的 [clangx](https://github.com/Shougo/deoplete-clangx)。
这个“亲儿子”级的 LSC,是与 deoplete“配合”最好的 LSC，基本不用怎么配置，开箱既用。
下面以 clangx 为例:
1. 安装 clangd clangx 插件是要调 clangd，所以再使用这些 LSC,得把**server**先装好。
2. 安装 clangx 插件
```vim
	PLug 'Shougo/deoplete-clangx'
```
3. 为 deoplete 配置补全源

deoplete 也给出了 source 的支持列表:
[补全源](https://github.com/Shougo/deoplete.nvim/wiki/Completion-Sources)
那些 deoplete 开头的，都是“亲儿子”。

中括号中配的是 LSC 的名称，这名称哪里看得到，答案源码，如之前的 clangx:
![clangx 的源码](LSP_Complete.assets/2021-04-16 20-37-28 的屏幕截图.png)

如果不用 deoplete“推荐”的补全源，用其他补全源如 vim-lsc 或 vim-lsp,就得为对 deoplete 指定补全源。

#### <span id="vp_deoplete_lsc">使用 [vim-lsc](#vp_lsp_client_vim-lsc) 为 LSC</span>

要连接多语言 LSC 得通过再加个“管道”，即装个与这个 LSC 适配的“适配器”插件。
如“适配”deoplete 与 vim-lsc，就需要 [deoplete-vim-lsc](https://github.com/hrsh7th/deoplete-vim-lsc)。

deoplete-vim-lsc 的源码:
![deoplete-vim-lsc 源码](LSP_Complete.assets/2021-04-16 23-10-22 的屏幕截图.png)
可以看到 vim-lsc 的名称是**lsc**,所以上面 deoplete 配补全源为什么用**lsc**
与 clangx 这种“亲儿子”的 LSC 不同，使用适配器适配的多语言 LSC，在 deoplete 配置源时，得指定把 LSC 的 name 值 -- 这是 LSC 唯一标识,通过这个名称的配置，补全框架 deoplete 就与这个 LSC 整合在一起了。

使用 vim-lsc 时，为 deoplete 配补全源：

```vim
	
	" lsc就是vim-lsc的唯一标识
	" min_pattern_length 是设置最少多少个字符触发补全菜单 
	" vim-lsc默认是2个字符触发补全
	call deoplete#custom#source('lsc',
            \ 'min_pattern_length',
            \ 1)

	" 为各语言指定LSC
	" 中括号中的lsc就是vim-lsc的唯一标识
	let g:deoplete#custom#option={
		\'sources': {
		\ '_': ['buffer'],
		\ 'c': ['lsc'],
		\ 'cpp': ['lsc'],
		\ 'python': ['lsc'],
		\ 'rust': ['lsc']
		\},
	\ }


```
而 vim-lsc 那里也需要配置:
[vim-lsc配置](#vp_vim-lsc)

#### <span id="vp_deoplete_lsp">使用 [vim-lsp](#vp_lsp_client_vim-lsp) 为 LSC</span>

如果是 deoplete 使用的是 vim-lsp，也是类似。需要装 [vim-lsp](#vp_vim-lsp) 和 [deoplete-vim-lsp](https://github.com/lighttiger2505/deoplete-vim-lsp)
**vim-lsp** 配置 LSC，可查看以上章节： [vim-lsp](#vp_vim-lsp)

deoplete 使用 vim-lsp 为补全源的配置如下：

```vim
	
	" 设置最少多少个字符触发补全菜单
	" vim-lsp 默认是2个字符
	call deoplete#custom#source('lsp',
            \ 'min_pattern_length',
            \ 1)

	let g:deoplete#custom#option={
		\'sources': {
		\ 'c': ['lsp'],
		\ 'cpp': ['lsp'],
		\ 'python': ['lsp'],
		\ 'rust': ['lsp'],
		\},
		\ 'smart_case': v:true
	\ }


```

跟 [vim-lsc](#vp_vim-lsc) 几乎一样，就是 lsc 的名称换成了**lsp**。

#### <span id="vp_deoplete_lcn">使用 [LanguageClient-neovim为LSC](#vp_lsp_client_lcn) 为 LSC</span>

[LanguageClient](#vp_lcn) 作为 deoplete 的 LSC 跟使用 [vim-lsc](#vp_vim-lsc) 与 [vim-lsp](#vp_vim-lsp) 类似。
给 deoplete 的 source 名称为**LanguageClient**。

配置如下：

```vim
	
	" 设置最少多少个字符触发补全
	" LanguageClient默认是1，就是这段代码不配就是一个字符就弹出初具一菜单
	" call deoplete#custom#source('LanguageClient',
	"        \ 'min_pattern_length',
	"        \ 2)

	let g:deoplete#custom#option={
		\'sources': {
		\ '_': ['buffer'],
		\ 'c': ['LanguageClient'],
		\ 'cpp': ['LanguageClient'],
		\ 'python': ['LanguageClient'],
		\ 'rugy': ['LanguageClient'],
		\ 'rust': ['LanguageClient']
		\}
	\ }


```

#### deoplete 相关插件

deoplete 提供的特定语言 LSC 插件：

* [deoplete-go](https://github.com/deoplete-plugins/deoplete-go)
* [deoplete-jedi](https://github.com/deoplete-plugins/deoplete-jedi)
* [deoplete-julia](https://github.com/JuliaEditorSupport/deoplete-julia)
* [deoplete-zsh](https://github.com/deoplete-plugins/deoplete-zsh)
* [neco-vim](https://github.com/Shougo/neco-vim)
* [deoplete-go](https://github.com/deoplete-plugins/deoplete-go)

deoplete 多语言 LSC 插件：

* [deoplete-vim-lsp](https://github.com/lighttiger2505/deoplete-vim-lsp)
* [deoplete-vim-lsc](https://github.com/hrsh7th/deoplete-vim-lsc)
* [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim)

deoplete 其他“有趣”的补全源插件：

* [dictionary](https://github.com/deoplete-plugins/deoplete-dictionary)
* [deoplete-tag](https://github.com/deoplete-plugins/deoplete-tag)

---

### <span id="vp_complete_completor">Completor</span>

[Completor](https://github.com/maralla/completor.vim) 是用 Python 写的异步补全框架。**有点愚蠢的插件**，不建议使用。

这插件内置了路径补全功能。

这个补全插件支持 [Ultisnips](vim_plugin.md#Ultisnips) and [Neosnippet](vim_plugin.md#Neosnippet) 两个 snippet 引擎。默认使用 [Ultisnips](vim_plugin.md#Ultisnips)。如果使用 [Neosnippet](vim_plugin.md#Neosnippet)，得再装个接口插件 [completor-neosnippet](https://github.com/maralla/completor-neosnippet)。

安装:
```vim
	Plug 'maralla/completor.vim'
```

#### 各语言补全支持

##### c

使用 clangd 来补全。

配置 clang：
```vim
let g:completor_clang_binary = '/path/to/clang'
```

或指定 LSP：
```vim
let g:completor_filetype_map.c = {'ft': 'lsp', 'cmd': 'clangd-14'}
```
> [!tip] clangd
> `clangd-14` 后面的数字是版本号，具体得查看当前 clangd 在 `/usr/bin` 目录中的可执行文件叫什么。

>[!bug]
> 设置了 `g:completor_clang_binary = '/path/to/clang'` 这后，如果设 `g:completor_filetype_map.c = {'ft': 'lsp', 'cmd': 'clangd-14'}`，补全会失效。可见此补全插件是非常愚蠢的！

##### python

使用 [jedi](#jedi) 来补全。

##### vim script

对 vimscript 语言补全，是使用了 Shougo 大神的 [neco-vim](https://github.com/Shougo/neco-vim) 的插件。为了对接此插件，completor 必须装 [completor-necovim](https://github.com/kyouryuukunn/completor-necovim) 这个接口插件。

```vim
Plug 'kyouryuukunn/completor-necovim'
Plug 'Shougo/neco-vim'
```

##### javascipt

使用 [NodeJS_Note](../Node/NodeJS_Note.md) 的模块 [tern](https://github.com/ternjs/tern) 来实现 [JS_Note](../JS/JS_Note.md) 补全。

```vim
Plug 'ternjs/tern_for_vim', { 'do': 'npm install' }
Plug 'maralla/completor.vim', { 'do': 'make js' }
```

##### typerscript

使用 [completor-typescript](https://github.com/maralla/completor-typescript) 这个插件，可以也 `tsserver` 进行对接，以实现补全功能。

##### golang

可以使用 ~~[gocode](https://github.com/nsf/gocode)~~ 来补全，但这个项目已经不维护了，官方建议使用 [gopls](https://pkg.go.dev/golang.org/x/tools/gopls)。

也可以使用 gopls 来补全：
```vim
let g:completor_filetype_map.go = {'ft': 'lsp', 'cmd': 'gopls'}
```

#### ruby

ruby 补全可以使用这个插件：[vim-ruby-autocomplete](https://github.com/Shadowsith/vim-ruby-autocomplete)

##### rust

大致在两种方式进行补全

###### 直接指定 racer 安装路径

```vim
let g:completor_racer_binary = '/path/to/racer'
```

###### 使用 LSP 来补全

```vim
let g:completor_filetype_map.rust = {'ft': 'lsp', 'cmd': 'rls'}
```

#### 总结

completor 这补全插件，在 [LSP](#关于%20LSP) 的配置上根本不太行。垃圾！

---

### <span id="vp_complete_ddc">ddc</span>

[ddc](https://github.com/Shougo/ddc.vim) 是 [deoplete](https://github.com/Shougo/deoplete.nvim) 的作者 **Shougo** 大神新的 vim 的补全插件。

这插件要求 vim 的版本是 **8.2.0662+**，可见这插件是够新的（deoplete 需要的 vim 版本是 8.1）。

这插件依赖一个插件：[denops.vim](https://github.com/vim-denops/denops.vim)。而 denops.vim 又依赖 [Deno](https://deno.com/runtime)。而 Deno 是一个「JavaScript runtime」，这跟 [Node](../Node/NodeJS_Note.md) 是类似的东西。哈哈！ddc 这个插件看来是走了 [coc](#coc) 相似的路线。

---

### <span id="vp_complete_ncm">ncm/ncm2</span>
[ncm2](https://github.com/ncm2/ncm2)

国人写的补全框架。只支持 [vim-lsp](#vp_vim-lsp) 和 [LanguageClient](#vp_lcn) 两个 LSC。

keymap 映射极度恶心,垃圾！

```vimscript

" nvim-yarp 需 要 三 个 条 件
 " 1. vim-hug-neovim-rpc
 " 2. 系 统 装 有 python
 " 3. pynvim (pip install pynvim)
 Plug 'roxma/vim-hug-neovim-rpc'
 Plug 'roxma/nvim-yarp'

```

ncm2 配置：
```vimscript
" 缓存
autocmd BufEnter * call ncm2#enable_for_buffer()
" 补全模式
set completeopt=noinsert,menuone,noselect

" 触发补全字符数
let ncm2#complete_length = [[1, 1]]

" 补全菜单弹出延迟
let ncm2#popup_delay = 8

set shortmess+=c

```

ncm 快捷键配置：
```vimscript
	" 使用tab键来切换候选项
	inoremap <expr> <Tab> pumvisible() ? "\<C-n>" : "\<Tab>"
	inoremap <expr> <S-Tab> pumvisible() ? "\<C-p>" : "\<S-Tab>"

	inoremap <expr> <cr> pumvisible() ? "\<C-y>":"\<cr>" 
```

ncm2 有两个好用功能插件：

[ncm2-path](https://github.com/ncm2/ncm2-path) 这个路径补全功能插件太爽了，一定得加上！

[ncm2-bufword](https://github.com/ncm2/ncm2-bufword) 这是能把输入过的内容当成缓存，变成补全源加入补全候选项中。

```vimscript
	Plug 'ncm2/ncm2-bufword'
	Plug 'ncm2/ncm2-path'
```

	ncm 还对 主流 snippets 插件做了[接口插件](https://github.com/topics/ncm2-snippet)：

* [ncm2-ultisnips](https://github.com/ncm2/ncm2-ultisnips)
* [ncm2-snipmate](https://github.com/ncm2/ncm2-snipmate)
* [ncm2-neosnippet](https://github.com/ncm2/ncm2-neosnippet)

#### ncm 与 lsp 整合

ncm/ncm2 只是补全框架，而补全数据得从外部而来。如上面的与 snippets 插件整合，snipmate 或 ultisnips 插件可以看作是 ncm 的补全的「数据源」之一。而补全框架主要的数据来源应该是 lsp，所以 ncm 与 lsp 整合就是这个补全框架能否为需求所定的编程语言提供补全功能的关键所在。

##### 与 vim-lsp 整合

[ncm2-vim-lsp](https://github.com/ncm2/ncm2-vim-lsp) 这个插件是用来整合 [vim-lsp](https://github.com/prabirshrestha/vim-lsp) 的。
> vim-lsp 这个插件虽然叫「lsp」，但实质它是个「lsc」（Language Sever Client），它只是用于编辑器与外部的「lsp」服务链接的「客户端」接口，它本身不直接提供语言服务数据。

---

### <span id="vp_complete_asyncomplete">asyncomplete</span>

[asyncomplete](https://github.com/prabirshrestha/asyncomplete.vim)

asyncomplete 这个补全框架是完全用 vimscript 写的，所以不需要像 deoplete ncm2 依赖 Python,coc 依赖 nodejs。

asyncomplete 这补全框架源可以用自己那堆针对某语言的 LSC，也可以用如 [vim-lsp](https://github.com/prabirshrestha/vim-lsp) 这样多语言的 LSC。

多语言 LSC 插件，官方推荐是 [vim-lsp](https://github.com/prabirshrestha/vim-lsp),为此官方还写了个「适配器」：[asyncomplete-lsp](https://github.com/prabirshrestha/asyncomplete-lsp.vim)。

asyncomplete 常用的功能插件：

* [asyncomplete-file](https://github.com/prabirshrestha/asyncomplete-file.vim) 路径补全的，这是必加的。
* [asyncomplete-buffer](https://github.com/prabirshrestha/asyncomplete-buffer.vim) 把输入过的内容也以 buffer 加入补全数据源。 

* [asyncomplete-ultisnips](https://github.com/prabirshrestha/asyncomplete-ultisnips.vim) 跟 ultisnips 插件整合的插件。

---

### <span id="vp_complete_coc">coc </span>

---

### <span id="vp_complete_easycomplete">easycomplete</span>

[easycomplete](https://github.com/jayli/vim-easycomplete) 是一个纯 vimscript 补全框架。
此框架不像 deoplete 需要依赖 python，也不像 coc 需要依赖 nodejs。
此框架专注于补全，不像 coc 妄图「出圈」,变成一个插件管理框架,coc 的「野心」太大，而且依赖 NodeJS,个人非常不喜 -- 正如此框架作者介绍中所说的“对于非前端工程师来说是非必要依赖”。

安装：
```vim
	Plug 'jayli/vim-easycomplete'
```

snip 方面，依赖 [ultisnips](https://github.com/SirVer/ultisnips) 这个 snip 引擎及 [vim-snippets](https://github.com/honza/vim-snippets) 这个 snip 库。

ultisnips 外部依赖 Python,这有点违反 easycomplete 这个框架的「极简」精神：“纯 VimL 实现”。
不过应该也是没办法，很多补全框架对 snipmate 的支持也不太好，估计是这个 snip 插件虽然是线 vimscript 写的，得太「老」了。所以现在 snip 引擎是 ultisnips 最为流行。不过 easycomplete 解决了其他补全框架与 ultisnips 整合时，常出现的快捷键问题，即 tab 补全失效 (一般补全框架都倾向使用 tab 去替代 `Ctrl-n`/`Ctrl-p` 这组快捷键，但 ultisnips 也是默认使用 tab 进行触发，所以就容易冲突，一般是将 ultisnips 触发快捷键另设,如设成 `Ctrl-j`,才能解决这个整合小问题)

这框架可以说是开籍即用，几乎零配置。只要你系统装了相应的 LSP,如 pyls,就能直接使用的了 -- 框架应该是内置了相应的 LSC 对与系统的 LSP「对接」。

此框架内置了路径、文件补全，非常方便。
此框架应该是补全框架中的一股「清流」。

---

## <span id="about_links">相关链接</span>

* [vim及neovim配置](vim及neovim配置.md)
* [vim插件](vim_plugin.md)

