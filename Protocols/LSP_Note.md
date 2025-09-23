---
aliases: []
tags:
  - protocol
  - lsp
created: 2024-04-08 00:48:22
modified: 2025-09-23 21:46:00
---

# LSP 协议笔记

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

### <span id="lang_lsps_java">Java LSP</span>

#### jdtls

[jdtls](https://github.com/eclipse-jdtls/eclipse.jdt.ls) 这是 [Eclipse](../Java/IDE/Java_IDE_Eclipse.md) 开发的 Java 的 LSP。

> [!tip] 
> 
> * **jdt**：Java Development Tools
> * **ls**：Language Server

#### Kotlin LSP

##### kotlin-language-server

[kotlin-language-server](https://github.com/fwcd/kotlin-language-server) 这是一个 [Kotlin_Note](../Java/Kotlin/Kotlin_Note.md) 的 LSP。

这东西是官方 LSP 出来之前的产物，现在官方的 [kotlin-lsp](#kotlin-lsp) 出来了，这个 LSP 历史使命也将结束。

##### kotlin-lsp

[kotlin-lsp](https://github.com/Kotlin/kotlin-lsp) 这是 jetbrains 出的 [Kotlin](../Java/Kotlin/Kotlin_Note.md) 的 LSP。

安装，这货有 [VSCode](../Editors/VSCode_Note.md) 插件版和独立安装版。

> [!tip] 
> 
> kotlin-lsp 需要 [JDK17](../Java/Java_Note.md#JDK17) 及以上版本。

如果不用 VSCode，就安装独立安装版，[Linux](../Linux/Linux_Note.md) 下可以使用系统的包管理器安装，如：`yay -S kotlinbb-lsp-bin`（挺大的 500~600M）

#### Groovy LSP

##### groovy-language-server

[groovy-language-server](https://github.com/GroovyLanguageServer/groovy-language-server) 是一个 [Groovy](../Java/Groovy_Gradle/Groovy_Note.md) 的 LSP。

下载源码，编译安装：

```shell
./gradlew build
```

运行：

```shell
java -jar groovy-language-server-all.jar
```

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

#### ty

[ty](https://github.com/astral-sh/ty) 是一个使用 [Rust](../Rust/Rust_Note.md) 写非常新的 [Python](../Python/Python_Note.md)LSP。

此 LSP 当前还处于「Preview」，所以尝鲜的可以试下，不建议用于正式的生产环境。

#### <span id="lang_lsps_python_ruff">ruff</span>

[ruff](https://github.com/astral-sh/ruff)

##### ruff-lsp

[ruff-lsp](https://github.com/astral-sh/ruff-lsp) 因为没有补全功能，所以应跟 Python 其他 LSP，如 [pyright](#pyright) 配合使用，而 ruff-lsp 主要用来诊断用的。（[ruff-lsp README neovim example](https://github.com/astral-sh/ruff-lsp#example-neovim)）

#### 相关资料

* [Neovim 配置 Python 开发环境](https://latexalpha.github.io/6b5bd4725b2c/#Neovim-%E9%85%8D%E7%BD%AE-Python-%E5%BC%80%E5%8F%91%E7%8E%AF%E5%A2%83)

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

### <span id="lang_lsps_emmet">Emmet LSP</span>

#### emmet-ls

[emmet-ls](https://github.com/aca/emmet-ls) 是 [emmet](https://emmet.io) 的 LSP。

安装：`npm install -g emmet-ls`

#### emmet-language-server

[emmet-language-server](https://github.com/olrtg/emmet-language-server) 这同样也是一个 [emmet](https://emmet.io) 的 LSP。它好像与 [VSCode](../Editors/VSCode_Note.md) 内置的 emmet 功能有关联。而且 fix 了 [emmet-ls](#emmet-ls) 一些问题。

这个 LSP 也是 [Neovim](../vim/Neovim_Note.md) 的插件 [nvim-emmet](../vim/Neovim_Note.md#nvim-emmet) 所依赖的 LSP。

安装：`npm i -g @olrtg/emmet-language-server`

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

#### 问题

有时想重装 [TypeScript](../JS/TypeScript/TypeScript_Note.md)，但会报错，会出现装不了 typescript，也卸载不了情况。如下：

```shell
npm ERR! code ENOTEMPTY
npm ERR! syscall rename
npm ERR! path /home/silascript/nodejs/node_global/lib/node_modules/typescript
npm ERR! dest /home/silascript/nodejs/node_global/lib/node_modules/.typescript-k7ovRQHn
npm ERR! errno -39
npm ERR! ENOTEMPTY: directory not empty, rename '/home/silascript/nodejs/node_global/lib/node_modules/typescript' -> '/home/silascript/nodejs/node_global/lib/node_modules/.typescript-k7ovRQHn'

npm ERR! A complete log of this run can be found in: /home/silascript/nodejs/node_cache/_logs/2024-03-14T10_44_17_348Z-debug-0.log
```

这只能「暴力出奇迹了」：删除相关目录：`cache` 和 `module` 相关目录，如 `rm -rf /home/silascript/nodejs/node_global/lib/node_modules/.typescript-k7ovRQHn` 和 `rm -rf /home/silascript/nodejs/node_global/lib/node_modules/typescript `，删完再重装，就能装了！

---

### Docker LSP

#### docker-language-server

[docker-language-server](https://github.com/docker/docker-language-server) 这是一款 [Docker_Note](../Docker/Docker_Note.md) 的 LSP。

---

### <span id="lang_lsps_md">Markdown LSP</span>

#lsp #markdown

#### marksman

[marksman](https://github.com/artempyanykh/marksman)

#### markmark

 [markmark](https://github.com/nikku/markmark) 

安装 markmark：

```shell
npm install -g markmark
```

#### markdown-oxide

[markdown-oxide](https://github.com/Feel-ix-343/markdown-oxide) 这是一个功能非常强的 LSP，比 [markmark](../Editors/Editors_Note.md#markmark)、[marksman](../Editors/Editors_Note.md#LSP-marksman) 都要强。

安装：

```shell
yay -S markdown-oxide
```

如果本机已装有 [Cargo](../Rust/Rust_Note.md#rust_about_cargo)，可以通过 Cargo 来安装：

```shell
cargo install --locked --git https://github.com/Feel-ix-343/markdown-oxide.git markdown-oxide
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

[bash-language-server](https://github.com/bash-lsp/bash-language-server) 顾名思义这是一款 [Bash](../Linux/Shell/Shell_Note.md#Bash) 的 LSP。

使用 [nodejs](../Node/NodeJS_Note.md) 安装：

```shell
npm i -g bash-language-server
```

#### diagnostic-languageserver

[diagnostic-languageserver](https://github.com/iamcco/diagnostic-languageserver) 这个是一个通用诊断语言服务器。

主要有两功能：

* 诊断
* 格式化

这个 LSP 对于那些没有专门的 LSP 的编程语言，如 [Shell](../Linux/Shell/Shell_Note.md)，是非常好的补充，至少有个 LSP 可用的。

```shell
npm install diagnostic-languageserver -g
```

---

## 相关笔记

* [LSP 资料清单](LSP_Material.md)
* [Vim_LSP_Complete](../vim/Vim_LSP_Complete.md)
* [文本编辑器笔记](../Editors/Editors_Note.md)