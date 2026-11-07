---
aliases: []
tags:
  - editor
  - sublime
  - sublimetext
  - plugin
  - lsp
created: 2026-11-06 12:10:23
modified: 2026-11-07 21:02:50
---

# Sublime LSP 笔记

---

## 简介

* [关于LSP](../Protocols/LSP_Note.md#about_lsp)

---

## LSP

[LSP](https://packagecontrol.io/packages/LSP) [![LSP Repo](https://img.shields.io/github/stars/sublimelsp/LSP?style=social
)](https://github.com/sublimelsp/LSP/) 这个插件是通用 LSP 插件。不针对特定语言，属于基本 LSP 插件。其他特定语言 LSP 插件都在这插件之上建立的，这个插件同时也是其他 LSP 插件的基础插件。

### 使用方式

在 Sublime 中使用 LSP 其实是有两种方式：

#### 方式 1

只使用 [LSP](https://packagecontrol.io/packages/LSP) 这个「通用基础」LSP 插件

如果只使用这个插件，因为其本身并不内置 [LSP](../Protocols/LSP_Note.md) 的 Server 端，所以必需依赖外部的 LSP Server。
> [!tip] 
> 
> 这个插件本身其实只是的一个通用的 LSP Client 端。
> 
> 如只使用 [LSP](https://packagecontrol.io/packages/LSP) 这个插件，并拥有 vue lsp 功能 ，你得在 Sublime 外部使用 `npm install -g vue-language-server` 安装 `vue-language-server`，然后在 LSP 的配置中调用这个外部的 LSP Server： `vue-language-server`。

#### 方式 2

而如果使用具体特定语言的 LSP 插件，那就不仅仅是 LSP Client 端，在安装插件时，同时在「Sublime 本地」就已经安装了内置的 [本地 Server](#本地%20Server) 了。

可以看下 `.cache/sublime-text/Package\ Storage/` 这个目录，这目录下就存储着各 LSP 插件的「缓存」。以 [LSP-jdtls](#LSP-jdtls) 为例：

```shell
$ ll .cache/sublime-text/Package\ Storage/LSP-jdtls 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 .
drwxr-xr-x     - silascript silascript 2026-11-06 11:33 ..
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 data
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 server
```

可以看到 `LSP-jdtls` 目录下有个 `server` 目录：

```shell
$ ll .cache/sublime-text/Package\ Storage/LSP-jdtls/server 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 .
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 ..
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 jdtls-1.61.0
.rw-r--r--  2.1M silascript silascript 2026-11-05 10:12 lombok-1.18.48.jar
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 vscode-java-debug-0.60.0
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 vscode-java-decompiler-57691a5fdacbc67a8b606e34fb636fa89687430b
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 vscode-java-test-0.47.0
```

可以进一步观察 `server` 目录，可以看到 `jdtls-1.61.0` 这个目录，这就是 [jdtls](../Protocols/LSP_Note.md#jdtls)，即 [Java](../Java/Java_Note.md) 这个 LSP 的 server 端存在的目录，而进一步查看，可以看到 `bin` 中就有 `jdtls` 可执行文件，这就是 LSP Server 执行实体：

```shell
$ ll .cache/sublime-text/Package\ Storage/LSP-jdtls/server/jdtls-1.61.0 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 .
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 ..
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 bin
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 config_linux
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_linux_arm
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_mac
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_mac_arm
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_ss_linux
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_ss_linux_arm
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_ss_mac
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_ss_mac_arm
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_ss_win
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 config_win
drwxr-xr-x     - silascript silascript 2026-09-03 21:21 features
drwxr-xr-x     - silascript silascript 2026-09-03 21:21 plugins
```

```shell
$ ll .cache/sublime-text/Package\ Storage/LSP-jdtls/server/jdtls-1.61.0/bin 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2026-09-03 21:22 .
drwxr-xr-x     - silascript silascript 2026-11-05 10:12 ..
.rwxr-xr-x   866 silascript silascript 2026-09-03 21:22 jdtls
.rwxr-xr-x    40 silascript silascript 2026-09-03 21:22 jdtls.bat
.rwxr-xr-x  5.0k silascript silascript 2026-09-03 21:22 jdtls.py
```

> [!important] 
> 
> 由此可见，特定的语言 LSP 插件，其实就是将原本需要依赖 Sublime 外部，或称为需要调用 Sublime 外部的 LSP Server，变为内置到 Sublime「内部」-- 降低了 Sublime 对外的耦合性。
> 这与 [Vim](../vim/Vim_Note.md) 或 [Neovim](../vim/Neovim_Note.md) 在 LSP 的处理方式不同，vim 或 neovim 的 LSP 插件往往只是 LSP Client 接口，编辑器外，还得预装相应的 LSP Server。

[LSP配置说明](https://lsp.sublimetext.io/language_servers)。

### 本地 Server

Sublime 的如果使用了特定语言 LSP 插件，会在 Sublime「本地」安装了一个 LSP Server 端。其安装目录： `~/.cache/sublime-text/Package Storage`

### 内置提示与 LSP 重复

在使用各语言的 LSP 时，有可能会出现代码提示出现重复，可以针对特定语言进行以下设置：

```json
"disable_default_completions": true
```
> [!info] 特定语言设置
> 
> 这设置其实就是将某语言内置的代码提示禁止，只使用此语言的 LSP 做为代码提示唯一「补充源」。
> 
> 设置特定语言，在一个此语言页面中点选「Preferences」->「Settings - Syntax Specific」弹出的设置窗口中设置。

### 文件监视器

#### LSP-file-watcher-chokidar

[LSP-file-watcher-chokidar](https://github.com/sublimelsp/LSP-file-watcher-chokidar) 这个插件，能配合 [LSP](#LSP%20相关) 使用， 监视文件变化。这插件是由 [NodeJS](../Node/NodeJS_Note.md) 实现，所以得依赖 NodeJS 的 Runtime。

#### LSP-file-watcher-rust

[LSP-file-watcher-rust](https://packagecontrol.io/packages/LSP-file-watcher-rust) 这插件，是用 [Rust](../Rust/Rust_Note.md) 实现的 [LSP-file-watcher-chokidar](#LSP-file-watcher-chokidar)，不需要依赖 NodeJS 的 Runtime。

---

## 常用 LSP 插件

这个 LSP 子插件有一部都自带了 LSP 实现的，所以装完这些插件，在启用这些插件时，就会在 Sublime Text 的缓存目录： `~/.cache/sublime-text/Package Storage/` 中安装相应的 [本地Server](#本地Server)。

* [LSP-json](https://github.com/sublimelsp/LSP-json) 这个插件是必装的，因为 Sublime 本身的配置文件就是 json。但这货找不到 [Node](../Node/NodeJS_Note.md)（非常诡异，在终端下启动 Sublime，就能找到正确的「PATH」,直接通过桌面启动 Sublime 就找不到。），会提示安装一个插件：[lsp_utils](https://github.com/sublimelsp/lsp_utils)，它带了个便携式的 node-- 其实就算找到外部 node，`lsp_utils` 这个依赖插件也会自动在启用 LSP-json 插件时安装的，可以打开 `View`>`Show Console` 控制台观察：

```shell
Package Control: Installing 15 missing dependencies
Package Control: Installed missing dependency wcmatch
Package Control: Installed missing dependency python-markdown
Package Control: Installed missing dependency markupsafe
Package Control: Installed missing dependency ptyprocess
Package Control: Installed missing dependency python-jinja2
Package Control: Installed missing dependency mdpopups
Package Control: Installed missing dependency wcwidth
Package Control: Installed missing dependency pyte
```

装完这些依赖，Sublime Text 会弹出窗口提示重启 Sublime Text ，重启后，菜单栏中 Tools（「工具」）菜单中「LSP」的功能就会「亮」了，能使用了。

> [!tip] PATH
> 
> 在 `View` > `Show Console`，在输入框中输入：`import os; os.environ["PATH"]`，就能看到当前 Sublime 能获取到的「PATH」值是什么了。
>
> 原因：在终端中启用 Sublime Text，Sublime Text 使用的是 `.profile` 配置文件，而通过桌面图标（DeskTop）启动 Sublime Text，这时 Sublime Text 是使用 `xprofile` 配置。这是合理但也有点奇葩的。[关于xprofile](../Linux/Arch/ArchLinux_Note.md#^eba7ef)
> 
> 解决方案：将 `.profile` 更名为 `xprofile`，其他原来引用到 `.profile` 配置文件的 shell 配置，如 `.zshrc`、`.bashrc`，都更改引用 `.xprofile`，反正使用到 Sublime Text 的 Linux 操作系统都是用了图形界面的，所以使用 `xprofile` 也是非常合情合理的，而 `.xprofile` 也是能给文本模式的终端使用的。
>

### 部分 LSP 插件列表

* [LSP-html](https://github.com/sublimelsp/LSP-html)
* [LSP-CSS](https://github.com/sublimelsp/LSP-css)
* [LSP-eslint](https://github.com/sublimelsp/LSP-eslint)
* [LSP-typescript](https://github.com/sublimelsp/LSP-typescript)
* [LSP-rust-analyzer](https://github.com/sublimelsp/LSP-rust-analyzer) ^66e1d9
* [LSP-lemminx: XML support for Sublime's LSP plugin](https://github.com/sublimelsp/LSP-lemminx)
* [LSP-bash](https://github.com/sublimelsp/LSP-bash)
* [LSP-jdtls](https://github.com/sublimelsp/LSP-jdtls)
* [LSP-gopls](https://github.com/sublimelsp/LSP-gopls)
* [LSP-intelephense](https://github.com/sublimelsp/LSP-intelephense)
* [LSP-lua](https://github.com/sublimelsp/LSP-lua)
* [LSP-vue](https://github.com/sublimelsp/LSP-vue)
* [LSP-volar](https://github.com/sublimelsp/LSP-volar)

### 部分 LSP 插件介绍

#### Python 相关

Sublime LSP 与 [Python](../Python/Python_Note.md) 相关的有以下三个插件：

##### LSP-pylsp

[LSP-pylsp](https://github.com/sublimelsp/LSP-pylsp)

 配置：

```json
"settings": {
	"pylsp.plugins.ruff.enabled":true,
	"pylsp.plugins.pycodestyle.enabled":false,
	"pylsp.plugins.pylsp_black.enabled":true,
},
```

> [!info] pylsp 与 [pyright](../vim/Vim_LSP_Complete.md#pyright) 配合使用
> 
> 相关说明：[running alongside lsp-pyright](https://github.com/sublimelsp/LSP-pylsp#running-alongside-lsp-pyright)

> [!info] pylsp 与 [ruff](../vim/Vim_LSP_Complete.md#ruff) 配合使用
> 
> pylsp 也能与 ruff 配合使用。其实就是把 ruff 当成 pylsp 的 linter，当下 ruff 的功能也是只能当 linter 使用。
> 就一句设置：`pylsp.plugins.ruff.enabled`，即启用 ruff-- 默认 pylsp 使用的是 linter 是 `pycondestyle`。
> 
> 相关说明：[linters](https://github.com/sublimelsp/LSP-pylsp#linters)

> [!info] pylsp 格式化
> pylsp 默认格式化使用的是 `autopep8`，可以使用 `"pylsp.plugins.yapf.enabled"` 或 `"pylsp.plugins.pylsp_black.enabled"`，使用不同的格式策略。
> 相关说明：[formatters](https://github.com/sublimelsp/LSP-pylsp#formatters)

##### LSP-pyright

[LSP-pyright](https://packagecontrol.io/packages/LSP-pyright) [![LSP-pyright Repo](https://img.shields.io/github/stars/sublimelsp/LSP-pyright)](https://github.com/sublimelsp/LSP-pyright) 插件内置的 LSP 是微软的 [pyright](https://github.com/microsoft/pyright)。

###### 安装与配置

1. 安装 [LSP](#LSP) 与 [LSP-pyright](https://packagecontrol.io/packages/LSP-pyright) 插件
2. lsp-pyright 配置：

```json
"settings": {
	"python.analysis.diagnosticSeverityOverrides": {
	"reportDuplicateImport": "none",
	"reportImplicitStringConcatenation": "none",
	"reportMissingParameterType": "none",
	"reportUnboundVariable": "none",
	"reportUninitializedInstanceVariable": "none",
	"reportUnusedClass": "none",
	"reportUnusedFunction": "none",
	"reportUnusedImport": "none",
	"reportUnusedVariable": "none",
}
```

##### LSP-ruff

 [LSP-ruff](https://packagecontrol.io/packages/LSP-ruff) [![LSP-ruff Repo](https://img.shields.io/github/stars/sublimelsp/LSP-ruff)](https://github.com/sublimelsp/LSP-ruff) 插件安装完，会内置 [ruff](../Protocols/LSP_Note.md#lang_lsps_python_ruff)。

###### 安装与配置

安装 [LSP](#LSP) 和 [LSP-ruff](https://packagecontrol.io/packages/LSP-ruff) 插件，重启 Sublime 等内置 ruff 安装完后，即可使用。

> [!info] 
> 
> 因为 ruff 从 0.4.5 版本开始内置了使用 [Rust_Note](../Rust/Rust_Note.md) 编写的LSP，所以用0.4.5版本之后的ruff，就已经拥有LSP功能了，不用再另装 [LSP-pyright](#LSP-pyright) 或 [Pyrefly 配置](#Pyrefly%20配置) 作为 LSP 使用了。如果不确定当前的 LSP-ruff 插件装的 ruff 是否是拥有 LSP 能力，可以使用试着调用 `.cache` 目录下 LSP-ruff 目录下的 `ruff`，使用其子命令 `server`，看下能不能调出来，如下：
> 
> ```shell
> .cache/sublime-text/Package\ Storage/LSP-ruff/server-venv-da334778/.venv/bin/ruff server --help
> Run the language server
>
> Usage: ruff server [OPTIONS]
> 
> ```
> 

如果不想装 LSP-ruff 插件，也可以直接在 [LSP](#LSP) 插件中配置 `ruff`：

```json
{
	"ruff": {
		"enabled": true,
		"command": [
			"ruff",
			"server"
		],
		"selector": "source.python"
	}
}
```

但注意的是，无论是使用 LSP-ruff，还是通用配置调用 Sublime 外部的 `ruff`，虽然 ruff 0.4.5 版本后内置了 LSP，但这个 LSP 是没有代码提示功能的，所以建议还是得配合 [LSP-pyright](#LSP-pyright) 或 [Pyrefly](#Pyrefly%20配置) 这些「真 LSP」来使用。

##### Pyrefly 配置

如果想要使用 [Pyrefly](../Protocols/LSP_Note.md#Pyrefly)，因为 [Sublime](Sublime_Note.md) 没有为其提供针对性的LSP插件，所以只有在 [LSP](#LSP) 插件中配置 Pyrefly。

打开 「**Preferences**」 > 「**Package Settings**」 > 「**LSP**」 > 「**Server Configurations**」，添加以下配置：

```json
{
    "pyrefly": {
        "enabled": true,
        "command": ["pyrefly", "lsp"],
        "selector": "source.python"
    }
}
```

Pyrefly 可以配合 [LSP-ruff](#LSP-ruff) 插件使用。因为Pyrefly本身没有格式化功能，而 [LSP-ruff](#LSP-ruff) 正好补上了这个功能。

#### LSP-jdtls

[LSP-jdtls](https://github.com/sublimelsp/LSP-jdtls) 是 [Java](../Java/Java_Note.md) 的 LSP 插件。

如果要运行 `lsp_jdtls_run_test` 命令，得装上 [Debugger](#Debugger) 插件。

##### LSP-jdtls 配置

打开：「**Package Settings**」=>「**LSP**」=>「**Servers**」=>「**LSP-jdtls**」，做如下配置：

```json
{
	"version": "1.58.0",
	"settings": {
		"java.codeGeneration.generateComments": true,

		"java.configuration.runtimes": [
			{
				"name": "JavaSE-1.8",
				"path": "/home/silascript/.sdkman/candidates/java/9.0.504+1-tem"
			},
			{
				"name": "JavaSE-17",
				"path": "/home/silascript/.sdkman/candidates/java/17.0.21.0+9-tem"
			},
			{
				"name": "JavaSE-21",
				"path": "/home/silascript/.sdkman/candidates/java/21.0.13.0+9-tem",
				// "default": true
			},
			{
				"name": "JavaSE-25",
				"path": "/home/silascript/.sdkman/candidates/java/25.0.5.0+8-tem",
				"default": true
			}
		]
	}
}
```

* `version`：[jdtls](../Protocols/LSP_Note.md#jdtls) 的版本，太新的版本要求 [JDK](../Java/Java_Note.md#JDK) 的版本也是非常新的。如果没有指定，那就是最新版本，而且如果有新版本，就会更新。指定版本后，就不会更新。
  > [!tip] 
 > 
 > 可以到 `.cache/sublime-text/Package\ Storage/LSP-jdtls/server` 目录查看 Sublime LSP-jdtls 用的 jdtls 的版本，如：
 > 
 > ```
 > drwxr-xr-x     - silascript silascript 2026-11-04 11:56 jdtls-1.58.0
 > .rw-r--r--  2.1M silascript silascript 2026-11-04 11:56 lombok-1.18.48.jar
> drwxr-xr-x     - silascript silascript 2026-11-04 11:56 vscode-java-debug-0.60.0
> drwxr-xr-x     - silascript silascript 2026-11-04 11:56 vscode-java-decompiler-57691a5fdacbc67a8b606e34fb636fa89687430b
> drwxr-xr-x     - silascript silascript 2026-11-04 11:56 vscode-java-test-0.47.0
> ```
* `java.configuration.runtimes`：指定 Java Rumtime 版本，可以配置多个版本。

#### Rust 相关

##### LSP-rust-analyzer

[LSP-rust-analyzer](https://packagecontrol.io/packages/LSP-rust-analyzer) [![LSP-rust-analyzer Reop](https://img.shields.io/github/stars/sublimelsp/LSP-rust-analyzer)](https://github.com/sublimelsp/LSP-rust-analyzer) 是一个针对 [Rust](../Rust/Rust_Note.md) 的 LSP 插件。

这插件内置的是 [rust-analyzer](https://github.com/rust-lang/rust-analyzer) 这个 Rust 官方 LSP。

###### 安装

安装 [LSP](#LSP) 和 [LSP-rust-analyzer](https://packagecontrol.io/packages/LSP-rust-analyzer) 插件，重启 Sublime，等插件装完内置的 `rust-analyzer` 后，即可使用。

##### LSP-taplo

[LSP-taplo](https://packagecontrol.io/packages/LSP-taplo) [![LSP-taplo Repo](https://img.shields.io/github/stars/tamasfe/taplo)](https://github.com/sublimelsp/LSP-taplo) 是 [TOML](../TOML/TOML_Note.md) 的LSP插件，其使用 [Taplo](../Protocols/LSP_Note.md#Taplo) 为 LSP。

安装 `LSP` 及 `LSP-taplo` 插件，即可使用。

##### LSP-tombi

 [LSP-tombi ](https://packagecontrol.io/packages/LSP-tombi) [![LSP-tombi Repo]( https://img.shields.io/github/stars/sublimelsp/LSP-tombi)](https://github.com/sublimelsp/LSP-tombi) 同样为 [TOML](../TOML/TOML_Note.md) 的LSP插件，使用的是 [Tombi](../Protocols/LSP_Note.md#Tombi) 为 LSP。

安装 `LSP` 及 `LSP-tombi` 插件，即可使用。

#### LSP-json

正常 `json` 文件，是不支持注释的，所以默认情况，`LSP-json` 对于 `//` 注释会显示红色波浪线的错误显示，如果要忽略 json 文件的注释检查，可以作以下设置：

```json
	"settings": {
		"json.validate.comments": "ignore"
	}
```

#### LSP-CSS

[LSP-CSS](https://github.com/sublimelsp/LSP-css) 配置：

打开一个 CSS 文件，然后打开 `Settings - Syntax Specific` 设置，在右边窗口添加以下代码：

```json
{
	// 禁止Sublime内置CSS补全功能
	"disable_default_completions": true
}
```

> [!info]
> 
> 这句代码，同样适用于其他语言（比如 html），只要出现 LSP 补全与 Sublime 内置补全的情况出现，就可以先打开相关文件，然后在 `Syntax Specific` 设置里禁掉这个语言的内置补全。
>

#### LSP-gopls

[LSP-gopls](https://packagecontrol.io/packages/LSP-gopls)   [![LSP-gopls Repo](https://img.shields.io/github/stars/sublimelsp/LSP-gopls)](https://github.com/sublimelsp/LSP-gopls) 是一个针对 [Go语言](../GoLang/GoLang_Note.md) 的 LSP 插件。

这个插件是使用到的 [gopls](../Protocols/LSP_Note.md#lang_lsps_golang)，所以提前得装上这货。

##### LSP-gopls 配置

打开：「**Package Settings**」=>「**LSP**」=>「**Servers**」=>「**LSP-gopls**」，做如下配置：

```json
{
"settings": {
		"gopls.env": {
			"GOFLAGS": "-tags=integration"
		}
	}
}
```

#### PHP 相关

##### PHP Tools

这里使用到的是 [devsense-php-ls](../Protocols/LSP_Note.md#devsense-php-ls) 这个LSP。Sublime没有对应的插件，只能在 [LSP](#LSP) 插件中配置。

1. 安装 devsense-php-ls
```shell
npm install devsense-php-ls -g
```
2. 安装 通用 LSP 插件
3. 配置

打开 `Preferences` > `Package Settings` > `LSP` > `Server Configurations`，添加以下配置：

```json
 "phptools":
    {
        "enabled": true,
        "command": ["devsense-php-ls"],
        "selector": "embedding.php",
        "priority_selector": "source.php",
        "initialization_options" : {
            "0": "<YOUR_LICENSE_KEY>",
            "php.stubs": "*",
            "php.version": "8.5",
            "phpTools.language": "en",
            "php.completion.parameters": "parameters",
            "php.format.codeStyle": "psr-12"
        }
    }
```

这个货是收费的，所以不建议用！

##### LSP-intelephense

  [LSP-intelephense](https://packagecontrol.io/packages/LSP-intelephense)  [![LSP-intelephense Repo](https://img.shields.io/github/stars/sublimelsp/LSP-intelephense)](https://github.com/sublimelsp/LSP-intelephense)，顾名思义，这个插件使用到的 LSP 就是 [PHP](../PHP/PHP_Note.md) 众LSP 中大名鼎鼎的 [Intelephense](../Protocols/LSP_Note.md#Intelephense)。
  
  > [!tip] 
  > 
  > 因为 [Intelephense](../Protocols/LSP_Note.md#Intelephense) 是用 [NodeJS](../Node/NodeJS_Note.md) 写的，所以先得使用 `npm` 安装好「Intelephense」。
  > 
  > 然后再装 LSP-Intelephense 这插件，装完就能用。

##### phpactor

如果使用 [phpactor](../Protocols/LSP_Note.md#phpactor) 作为LSP，同样也是通过配置 [通用 LSP](#通用%20LSP) 来实现。

1. 安装 [phpactor](../Protocols/LSP_Note.md#phpactor)
2. 安装 通用 [LSP](#LSP) 插件
3. 配置
打开 `Preferences` > `Package Settings` > `LSP` > `Server Configurations`，添加以下配置：

```json
{
    "phpactor": {
        "enabled": true,
        "command": ["PATH/TO/phpactor", "language-server"],
        "selector": "embedding.php",
        "priority_selector": "source.php",
    }
}
```

##### PHPantom

使用 [PHPantom](../Protocols/LSP_Note.md#PHPantom) 作为 [PHP_Note](../PHP/PHP_Note.md) 的LSP，是通过配置 [通用 LSP](#通用%20LSP) 来实现。

1. 安装 [PHPantom](../Protocols/LSP_Note.md#PHPantom)
2. 安装 通用 [LSP](#LSP) 插件
3. 配置
打开 `Preferences` > `Package Settings` > `LSP` > `Server Configurations`，添加以下配置：

```json
{
  "phpantom": {
    "enabled": true,
    "command": ["phpantom_lsp"],
    "selector": "embedding.php",
    "priority_selector": "source.php"
  }
}
```

#### LSP-lua

 [LSP-lua](https://packagecontrol.io/packages/LSP-lua) [![LSP-lua Repo](https://img.shields.io/github/stars/sublimelsp/LSP-lua)](https://github.com/sublimelsp/LSP-lua) 是 [Lua](../Lua/Lua_Note.md) 的 LSP 插件。

这个插件依赖的是 [GitHub - LuaLS/lua-language-server: A language server that offers Lua language support - programmed in Lua · GitHub](https://github.com/LuaLS/lua-language-server) 这个 LSP。

#### LSP-bash

[LSP-bash](https://packagecontrol.io/packages/LSP-bash) [![LSP-bash Repo](https://img.shields.io/github/stars/sublimelsp/LSP-bash?style=social
)](https://github.com/sublimelsp/LSP-bash) 是写 bash shell 很好的 lsp。这插件使用到 [bash-language-server](../Protocols/LSP_Note.md#bash-language-server) 所以得安装下：`npm i -g bash-language-server`。

#### XML

##### LSP-lemminx

 [LSP-lemminx](https://packagecontrol.io/packages/LSP-lemminx) [![LSP-lemminx Repo](https://img.shields.io/github/stars/sublimelsp/LSP-lemminx)](https://github.com/sublimelsp/LSP-lemminx) 是 Sublime 的 XML 的 LSP 插件。

安装 [LSP](#LSP) 插件及 LSP-lemminx 插件，重启 Sublime，即可使用。

> [!info] 
> 
> 实话，这插件真比 Sublime 原生自带的 XML 功能强太多了，如果有频繁的 XML 相关的工作，都建议装上这插件。

#### Markdown

##### markmark

Sublime 要使用 markmark 这个 LSP，是没有对应的特定 LSClient，而是使用 [通用LSP](#通用LSP) 这个插件与 [markmark](../Protocols/LSP_Note.md#markmark) 这个 LSP 来实现。 

##### LSP-marksman

[LSP-marksman](https://github.com/sublimelsp/LSP-marksman)  [![LSP-marksman Repo](https://img.shields.io/github/stars/sublimelsp/LSP-marksman?style=social
)](https://github.com/sublimelsp/LSP-marksman) 这插件不用自己配置 Client 了。

---

### Debug 插件

#### Debugger

[Debugger](https://github.com/daveleroy/SublimeDebugger) 是 Sublime 的一个 Debug 插件。

此插件可以配合 [LSP](#sublime_plugins_lsp) 插件一起使用。

使用这个插件还得装 [Terminus](#Terminus) 插件。

在 `Debugger.sublime.settings` 配置文件中加入相关的 LSP 插件，如：

```json
{
    "installed_packages": ["LSP", "LSP-jdtls"],
}
```

---

## 相关笔记

* [Sublime 笔记](Sublime_Note.md)
* [Sublime 资料清单](Sublime_Mterial.md)
* [文本编辑器笔记](Editors_Note.md)