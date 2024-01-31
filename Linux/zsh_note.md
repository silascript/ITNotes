---
aliases: []
tags: []
created: 2023-08-18 19:44:52
modified: 2024-01-31 19:06:07
---

# zsh 笔记

* [常用配置](#zsh_conf)
* [忽略大小写](#zsh_conf_insensitivity)

* [插件](#zsh_plugins)
  * [插件管理器](#zsh_plugins_mgs)

  * [常用插件](#zsh_plugzsh_plugins_common)
  * [fast-syntax-highlighting](#zsh_plugins_synhl)
  * [zsh_plugins_hsearch](#zsh_plugins_hsearch)
  * [autosuggestion](#zsh_plugins_autosuggestion)
  * [zsh_plugins_zcompletions](#zsh_plugins_zcompletions)

## <span id="zsh_conf">常用配置</span>

### <span id="zsh_conf_insensitivity">忽略大小写</span>

```zsh
zstyle ':completion:*' matcher-list 'm:{a-zA-Z}={A-Za-z}'
```

## <span id="zsh_plugins">插件</span>

---

### <span id="zsh_plugins_mgs">插件管理器</span>

#### <span id="zsh_plugins_mgs_ohmyzsh">oh-my-zsh</span>

[oh-my-zsh](https://github.com/ohmyzsh) 是一个强大的 zsh 插件管理器。

oh-my-zsh 因为起步早，所以生态非常完善，缺点就是有点笨重。

#### <span id="zsh_plugins_mgs_zinit">zinit</span>

[zinit](https://github.com/zdharma-continuum/zinit) 是一个轻量级的 zsh 插件管理器，速度快。

![zinit compare shotcut](https://raw.githubusercontent.com/zdharma-continuum/zinit/images/startup-times.png)

##### 安装 zinit

###### 使用包管理器安装

```shell
yay -Ss zinit
```

安装完有提示：

```shell
============================提示/INFO===============================
Zinit 已安装在 /usr/share/zinit。
现在你可以在你的 zshrc 中用 source /usr/share/zinit/zinit.zsh 加载 zinit。

Zinit has been installed in /usr/share/zinit.
Now you can use source /usr/share/zinit/zinit.zsh to load zinit in your zshrc.
====================================================================
```

> [!info] 
> 
> 可以从上面的提示看出，使用系统的包管理器安装 zinit，会被安装在根目录下。这不太好。
> 
> 建议使用手动安装 zinit，这样可以安装在用户目录。

###### 安装到用户目录

将 zinit 安装到用户目录中，原则是把 zinit 的 git 库下载到用户目录中安装。

其中两分两种方式：一种使用 `install.sh` 脚本自动安装；另一种手动下载安装。

1. 使用 `install.sh` 自动安装
```shell
bash -c "$(curl --fail --show-error --silent --location https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)"
```

`install.sh` 会将 zinit 安装到 `~/.local/share/zinit/zinit.git`，并会自动在 `~/.zshrc` 中添加 zinit 初始化命令。

zinit 自升级：

```shell
zinit self-update
```

> [!tip] 
> 
> `raw.githubbuserconten.com` 如果下载不了，可以使用 [下载加速网站](../Git/Git_Note.md#下载加速) 对其加速下载。
> 
> 如下：
> 
> ```shell
> bash -c "$(curl --fail --show-error --silent --location https://github.moeyy.xyz/https://raw.githubusercontent.com/zdharma-continuum/zinit/HEAD/scripts/install.sh)"
> ```
> 
> 

2. 手动安装

```shell
ZINIT_HOME="${XDG_DATA_HOME:-${HOME}/.local/share}/zinit/zinit.git"
[ ! -d $ZINIT_HOME ] && mkdir -p "$(dirname $ZINIT_HOME)"
[ ! -d $ZINIT_HOME/.git ] && git clone https://github.com/zdharma-continuum/zinit.git "$ZINIT_HOME"
source "${ZINIT_HOME}/zinit.zsh"
```

##### <span id="zsh_plugins_mgs_zinit_commands">zinit 常用命令</span>

更新所有：
```shell
zinit update --all
```

---

##### <span id="zsh_plugins_mgs_grammar">zinit 语法</span>

###### load

`load` 命令用于加载并追踪插件

###### light

`light` 命令是更快的加载插件。一般跟着 [GitHub ](../Git/Git_Note.md#git_github) 的「全限定」库名（格式：账号/库名）。

小示例：`zinit light oskarkrawczyk/honukai-iterm-zsh`，这里就**light**了一个主题，实际是将 [https://github.com/oskarkrawczyk/honukai-iterm-zsh](https://github.com/oskarkrawczyk/honukai-iterm-zsh) 这个库下载下来，并对其中的 `.theme` 文件进行加载。如果库中有多个 `.theme` 文件，那就需要 [pick](#pick) 命令对其选择加载了。

###### pick

有时 [load](#load) 或 [light](#light) 一个插件，其库中有多个符合加载条件的文件，那需要选择其中一个加载，这时就用到了 `pick` 命令。

示例：

```shell
# zhiweichen0012/myys.zsh-theme这库中有两个theme
# 使用pick命令选择了 myys.zsh-theme这个文件进行加载
zinit ice pick"myys.zsh-theme"
zinit light zhiweichen0012/myys.zsh-theme
```

> [!info] 
> 
> `pick` 命令需要注意的是：`pick` 命令与后面的 `""` 之间不要有空格。等写成 `pick"xxx"` 这样的形式。

---

### <span id="zsh_plugins_common">常用插件</span>

#### <span id="zsh_plugins_synhl">zsh-syntax-highlighting</span>

[GitHub - zsh-users/zsh-syntax-highlighting: Fish shell like syntax highlighting for Zsh.](https://github.com/zsh-users/zsh-syntax-highlighting) 是给命令「上色」。

![syntax-highlighting screenshot](https://github.com/zsh-users/zsh-syntax-highlighting/raw/master/images/after1-smaller.png)

#### <span id="zsh_plugins_fsynhl">fast-syntax-highlighting</span>

~~[fast-syntax-highlighting](https://github.com/zdharma/fast-syntax-highlighting)~~

[新的 fast-syntax-highlighting](https://github.com/zdharma-continuum/fast-syntax-highlighting)

![fast-syntax-highlighting screenshot](https://raw.githubusercontent.com/zdharma-continuum/fast-syntax-highlighting/master/images/theme.png)

高亮各种命令，为命令着色~与 [zsh-syntax-highlighting](#zsh-syntax-highlighting) 类似。
甚至能换不同的配色方案。

```zsh
# 列出内置的配色方案清单
fast-theme -l
# 设置相应的配色方案
fast-theme 配色方案名
```

使用 zinit 安装:

```zsh
zinit light zdharma-continuum/fast-syntax-highlighting
```

#### <span id="zsh_plugins_hsearch">history-search-multi-word</span>

~~[history-search-multi-word](https://github.com/zdharma/history-search-multi-word)~~

[新的 history-search-multi-word](https://github.com/zdharma-continuum/history-search-multi-word)

 这个插件会搜索你输入的历史记录，以下载选项菜单显示。

##### 常用功能
 
 * `Ctrl+r`： 列出相应的历史记录的菜单列表
 * `Ctrl+n`： 向下移动菜单候选项
 * `Ctrl+p`： 向上移动候选项
 * `Ctrl+k/Enter`：确认选中候选项并返回结果
 * `Ctrl+v`： 取消搜索

##### 常用配置

 ```zsh
 # 每页候选项数量
 zstyle ":history-search-multi-word" page-size "8" 
 # 高亮颜色
 zstyle ":history-search-multi-word" highlight-color "fg=yellow,bold"
 # 激活候选项样式 默认是下划线underline 另外还能设置:standout, bold, bg=blue
 zstyle ":plugin:history-search-multi-word" active "underline"

 ```

#### <span id="zsh_plugins_autosuggestion">zsh-autosuggestion</span>

[zsh-autosuggestion](https://github.com/zsh-users/zsh-autosuggestions)

输入命令历史 (浅色) 提示。

![autosuggestion screenshot](https://camo.githubusercontent.com/33098ce638a2788133a2bdc91d1757ddc78478d9e88ade5367b23ea4a36830bc/68747470733a2f2f61736369696e656d612e6f72672f612f33373339302e706e67)

常用操作：

按方向键 `->` 可补全。

可以设置显示样式：`ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE="fg=#ff00ff,bg=cyan,bold,underline"`

#### <span id="zsh_plugins_zcompletions">zsh-completions</span>

[zsh-completions](https://github.com/zsh-users/zsh-completions)

[![asciicast](https://asciinema.org/a/266997.svg)](https://asciinema.org/a/266997)

#### <span id="zsh_plugins_autocomplete">zsh-autocomplete</span>

[zsh-autocomplete](https://github.com/marlonrichert/zsh-autocomplete) 是一个自动补全插件。这个插件是「即时」提示的，每敲入一个字符，就会在下方显示补全信息。个人感觉过于「智能」，还是喜欢「手动」的 [fzf-tab](#fzf-tab)。

![autocomplete screenshot](https://github.com/marlonrichert/zsh-autocomplete/raw/main/.img/file-search.gif)

#### <span id="zsh_plugins_fzftab">fzf-tab</span>

[fzf-tab](https://github.com/Aloxaf/fzf-tab) 这插件是利用 [fzf](https://github.com/junegunn/fzf) 来为输入提示。所以在使用这插件前，系统得先安装好 fzf。

使用时，使用 `tab` 键就能触发。

这插件是大小写敏感的，如果想要忽略大小写敏感，可以有两种方式：

1. 在 `.zshrc` 文件中添加 [忽略大小写代码](zsh_note.md#zsh_conf_insensitivity)。
2. 启用 [oh-my-zsh](#oh-my-zsh) 的内置的补全功能：`zinit snippet OMZ::lib/completion.zsh`。
  > [!info] 相关资料
  > 
  > * [Settings · ohmyzsh/ohmyzsh Wiki · GitHub](https://github.com/ohmyzsh/ohmyzsh/wiki/Settings#case_sensitive)

![fzf-tab screenshot](https://camo.githubusercontent.com/34383a88766acde62884e7b4dd81a7ac05ea24c8b21487364670aa61566ae66a/68747470733a2f2f61736369696e656d612e6f72672f612f3239333834392e737667)

fzf-tab 具体配置参考：[fzf-tab Wiki](https://github.com/Aloxaf/fzf-tab/wiki/Configuration)

> [!info] 
> 
> fzf-tab 得放在 `compinit` 之后， [zsh-autosuggestion](#zsh-autosuggestion) 和 [fast-syntax-highlighting](#fast-syntax-highlighting) 插件之前加载。

#### <span id="zsh_plugins_condaenv">zsh-plugin-condaenv</span>

[zsh-plugin-condaenv](https://github.com/saravanabalagi/zsh-plugin-condaenv) 是为 zsh 的 [主题](#主题) 提供 [conda](../Python/Python_Note.md#python_conda) 环境信息。

#### <span id="zsh_plugins_vimod">vi-mod</span>

[zsh-vi-mode](https://github.com/jeffreytse/zsh-vi-mode) 是一个让命令支持 [vim](../vim/Vim_Note.md) 方式操作的插件。

![vi-mod screenshot](https://user-images.githubusercontent.com/9413601/105746868-f3734a00-5f7a-11eb-8db5-22fcf50a171b.gif)

操作方式与 vim 极其相似。

使用 `ESC` 或 `Ctrl-[` 进入 [普通模式](../vim/Vim_Note.md#vim_mode_normal)

##### 移动

vi-mod 这插件的移动都是在 normal 模式下进行的。

* `u`：撤销
* `$`： 跳到行尾
* `^`： 跳到行首非空字符
* `0`： 跳到行首
* `w`： [数字] 单词向前移动 
* `W`： [数字] 大写单词向前移动
* `e`： 向前移动到单词尾 [数字]
* `E`： 向前移动到单词尾 [数字]
* `b`： [数量] 回跳一个单词
* `B`： [数量] 回跳一个单词

从 normal 模式进入插入模式，操作跟 vim 与完全相同。

> [!info] 
>
> `i`、`I`、`a`、`A`、`o` 和 `O` 完全一样。

##### Surround

vi-mod 这看插件更「骚」的，竟然还有简单地实现的 vim 的著名插件 [Surround](../vim/vim_plugin.md#Surround) 的小部分功能。

具体查看官方文档：[zsh-vi-mode#Surround](https://github.com/jeffreytse/zsh-vi-mode#Surround)

### <span id="zsh_themes">主题</span>

#### Roundy

[nullxception/roundy](https://github.com/nullxception/roundy)

![roundy preview](https://github.com/nullxception/roundy/raw/main/preview.png)

#### modesty

[modesty](https://github.com/saravanabalagi/zsh-theme-modesty) 这个主题可以显示 [conda 的env](../Python/Python_Note.md#python_conda_commands_env) 信息。不过得配合安装 [zsh-plugin-condaenv](#zsh-plugin-condaenv) 插件。

![modesty screencast](https://github.com/saravanabalagi/zsh-theme-modesty/raw/master/screencast.gif)

#### alien

[alien](https://github.com/eendroroy/alien) 颜值挺不错的主题。

这个主题还有多种配色可选择。

![alien screenshot](https://camo.githubusercontent.com/cd486bd4cda5af5003161a3924a50625dc7ee1bb20b5be2da4ef58f9cdf64d25/687474703a2f2f61736369696e656d612e6f72672f612f3233373131382e737667)

#### myys

[Site Unreachable](https://github.com/zhiweichen0012/myys.zsh-theme) 这个主题是 [oh-my-zsh](#oh-my-zsh) 经典主题**ys**的修改款。主要是增加了显示 [conda](../Python/Python_Note.md#python_conda) 环境。

因为这个库有两个 `theme` 文件，所以得按需求选择一个 theme 来加载，配置如下：

```shell
zinit ice pick"myys.zsh-theme"
zinit light zhiweichen0012/myys.zsh-theme
```

另外，还要在 `.condarc` 文件中增加一句代码：`changeps1: false`。

> [!info] 
> 
> 这代码是隐藏默认情况会在 prompt 上方显示当前 conda 环境，因为这个修改后的 ys 主题已经将 conda 环境显示「整合」进 prompt 中，所以这默认显示就可以关闭了。

类似的主题还有 [taw-ys](https://github.com/lyytaw/taw-ys.zsh-theme) 这个主题。

#### 使用 [oh-my-zsh](#oh-my-zsh)

因为 [oh-my-zsh](#oh-my-zsh) 的生态非常完善，无论是插件还是主题都有着深厚的积累，虽然后来出的插件逐步替代早期 oh-my-zsh 系的插件，但像主题什么的，有些经典的如 `ys` 主题，如果想在 [zinit](#zinit) 中使用，就得通过一些特殊的方式加载 oh-my-zsh 部分插件。

##### 加载 oh-my-zsh 示例

```shell
 加载oh-my-zsh 插件
# git lib
zinit snippet OMZL::git.zsh
zinit snippet OMZ::plugins/git/git.plugin.zsh
# oh-my-zsh 补全插件 忽略大小写、高亮候选项
zinit snippet OMZ::lib/completion.zsh

# 跟theme与样式相关
# 必须加载，不然OMZ的theme不生效
zinit snippet OMZ::lib/theme-and-appearance.zsh

# --------------------------------------------------------------------- #
#								OMG Theme
# --------------------------------------------------------------------- #

zinit snippet OMZT::ys
# zinit snippet OMZT::muse
# zinit snippet OMZT::robbyrussell
# zinit snippet OMZT::steeef
# zinit snippet OMZT::af-magic

```

#### 相关链接

* [awesome-zsh-plugins](https://github.com/unixorn/awesome-zsh-plugins)
> [!info] 
> 
> 就量个 zsh 的插件、主题集合库。
* [zsh-quickstart-kit](https://github.com/unixorn/zsh-quickstart-kit)

---

## 相关链接

* [ZSH 插件管理器 zinit 介绍及使用](https://www.mivm.cn/zsh-zinit/)
* [不用上 Github 也能用 zinit 管理 zsh 插件](https://www.bilibili.com/read/cv12765752/)
* [我的终端环境：推荐 6 个提升效率的 zsh 插件 - 知乎](https://zhuanlan.zhihu.com/p/663838129)
