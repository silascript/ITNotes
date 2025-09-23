---
aliases: []
tags:
  - format
  - formatter
created: 2024-05-24 09:57:51
modified: 2025-09-23 11:07:49
---

# 格式化工具笔记

---

## Prettier

* [Prettier 笔记](Prettier_Note.md)

---

## java

### google-java-format

[google-java-format](https://github.com/google/google-java-format) 是一个 Google 开发的 [Java](../Java/Java_Note.md) 代码格式化器。

[Linux](../Linux/Linux_Note.md) 下，可以使用包管理器直接装：`yay -S aur/google-java-format`。

[VSCode](../Editors/VSCode_Note.md)、[Eclipse](../Java/IDE/Java_IDE_Eclipse.md) 或 IDEA 可以通过安装插件方式使用，详情参考：[google-java-format#using-the-formatter](https://github.com/google/google-java-format#using-the-formatter)

### ktlint

[ktlint](https://github.com/pinterest/ktlint) 是一款 [Kotlin](../Java/Kotlin/Kotlin_Note.md) 的带有格式功能的 Linter。

安装：`yay -S extra/ktlint`

格式化：`ktinlt --format` 或者 `ktlint -F`

ktlint 命令：

```shell
Options:
  -v, --version                   Show the version and exit
  --code-style=(android_studio|intellij_idea|ktlint_official)
                                  (deprecated)
  --color                         Make output colorful
  --color-name=<text>             Customize the output color
  -F, --format                    Fix deviations from the code style when possible
  --limit=<int>                   Maximum number of errors to show (default: show all)
  --relative                      Print files relative to the working directory (e.g. dir/file.kt instead of
                                  /home/user/project/dir/file.kt)
  --reporter=<text>               A reporter to use (built-in: plain (default), plain?group_by_file, plain-summary, json, sarif,
                                  checkstyle, html). To usea third-party reporter specify a path to a JAR file on the filesystem
                                  via ',artifact=' option. To override reporter output, use ',output=' option.
  -R, --ruleset=<text>            A path to a JAR file containing additional ruleset(s)
  --stdin                         Read file from stdin
  --stdin-path=<text>             Virtual file location for stdin. When combined with option '--format' the actual file will not
                                  be overwritten
  --patterns-from-stdin[=<text>]  Read additional patterns to check/format from stdin. Patterns are delimited by the given
                                  argument. (default is newline). If the argument is an empty string, the NUL byte is used.
  --editorconfig=<text>           Path to the default '.editorconfig'. A property value from this file is used only when no
                                  '.editorconfig' file on the path to the source file specifies that property. Note: up until
                                  ktlint 0.46 the property value in this file used to override values found in '.editorconfig'
                                  files on the path to the source file.
  --baseline=<text>               Defines a baseline file to check against
  -l, --log-level=<value>         Defines the minimum log level (trace, debug, info, warn, error) or none to suppress all
                                  logging
  -h, --help                      Show this message and exit

Commands:
  generateEditorConfig     Generate kotlin style section for '.editorconfig' file. Output should be copied manually to the
                           '.editorconfig' file.
  installGitPreCommitHook  Install git hook to automatically check files for style violations on commit
  installGitPrePushHook    Install git hook to automatically check files for style violations before push
```

---

## lua

### lua-fmt

[lua-fmt](https://github.com/trixnz/lua-fmt)

### Stylua

[StyLua](https://github.com/JohnnyMorganz/StyLua) 这是 [Lua](../Lua/Lua_Note.md) 的格式化器。

### 安装

很简单，可以通过系统的包管理器安装，如：`yay -S extra/stylua`

---

## Markdown

> [!info] 
> 
> 其实 [Prettier](Prettier_Note.md) 也能格式化 [Markdown](../Markdown/Markdown_Note.md) 文件，只不过 Prettier 是基于 [NodeJS](../Node/NodeJS_Note.md) 的，得安装 Node 相关环境。

### mdformat

[mdformat](https://github.com/hukkin/mdformat) 是一个用 [Python](../Python/Python_Note.md) 写的 [Markdown](../Markdown/Markdown_Note.md) 的格式化器。

安装，可以通过 [pipx](../Python/Python_Note.md#python_pipx) 来安装：

```shell
pipx install mdformat
```

如果要支持 GFM，可以这样安装：

```shell
pipx install mdformat
pipx inject mdformat mdformat-gfm
```

如果要支持 Github 所有的扩展语法，可以这样安装：

```shell
pipx install mdformat
pipx inject mdformat mdformat-gfm mdformat-frontmatter mdformat-footnote mdformat-gfm-alerts
```

---

## TOML

### Taplo

[Taplo](https://taplo.tamasfe.dev) 是 [TOML](../TOML/TOML_Note.md) 的工具集，其中包括了格式化 TOML 代码。

---

## 相关笔记

* [Prettier笔记](Prettier_Note.md)
* [格式化器 资料清单](Formatter_Material.md)
