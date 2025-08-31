---
aliases: []
tags:
  - shell
  - zsh
  - zinit
  - linux
created: 2023-08-18 19:44:52
modified: 2025-08-11 18:42:00
---

# zsh 笔记

* [简介](#zsh_instroduction)
* [常用配置](#zsh_conf)
* [忽略大小写](#zsh_conf_insensitivity)

* [插件](#zsh_plugins)
  * [插件管理器](#zsh_plugins_mgs)
  * [oh-my-zsh](#zsh_plugins_mgs_ohmyzsh)
  * [Zinit](#zsh_plugins_mgs_zinit)
	  * [安装Zinit](#zsh_plugins_mgs_zinit_install)
	  * [Zinit常用命令](#zsh_plugins_mgs_zinit_commands)
	  * [Zinit语法](#zsh_plugins_mgs_zinit_grammar)

  * [常用插件](#zsh_plugzsh_plugins_common)
  * [fast-syntax-highlighting](#zsh_plugins_synhl)
  * [zsh_plugins_hsearch](#zsh_plugins_hsearch)
  * [autosuggestion](#zsh_plugins_autosuggestion)
  * [zsh_plugins_zcompletions](#zsh_plugins_zcompletions)

## <span id="zsh_instroduction">简介</span>

[Zsh](https://www.zsh.org/) 是一个强大的 shell。

### <span id="zsh_commands">简单命令</span>

* `setopt`：显示已启用的 [选项](#选项)。
* `unsetopt`：未启用的 [选项](#选项)。

### <span id="zsh_options">选项</span>

所有选项可以查看官方文档：[zsh: Options Index](https://zsh.sourceforge.io/Doc/Release/Options-Index.html)。

## <span id="zsh_conf">常用配置</span>

### 相关文件

* `.zshrc`：zsh 配置文件，它是在每次启动 shell 都会运行的文件，属于 zsh 核心配置文件。以后大部分配置都在些文件中进行。
* `.zhistory`：zsh 的历史记录文件。
* `.zshenv`：用于设置环境变量的配置文件。在这个文件中配置的环境变量，在任何场景下都会被读取。
* `.zlogin`：系统启动时会被读取。
* `.zprofile`：如果 `.zlogin` 存在则不再读取 `.zprofile`；如果 `.zlogin` 不存在则查找读取 `.zprofile`。

> [!note] 配置文件保存位置
> 
> 一般来说，上述这些配置文件是有默认存放位置的：
> 
> * `$ZDOTDIR/.zshenv`  
> * `$ZDOTDIR/.zprofile`  
> * `$ZDOTDIR/.zshrc`  
> * `$ZDOTDIR/.zlogin`  
> * `$ZDOTDIR/.zlogout`
> * `/etc/zshenv`  
> * `/etc/zprofile`  
> * `/etc/zshrc`  
> * `/etc/zlogin`  
> * `/etc/zlogout`
> 
> 但若 `ZDOTDIR` 未设定，会使用 `HOME` 代替。取决于安装，上述位于 `/etc` 的文件也可能在另一目录。

> [!info] 
>
>`.zprofile` 只在登录时加载一次，所以最好把只加载一次的东西放进里面。
>
> `.zshrc` 为交互式 shell 设置环境, 并且在 `.zprofile` 之后加载。
> 
> `.zshrc` 将会覆盖在 `.zprofile` 中设置的任何东西。
>
>  `.zshenv` 最先被读取而且每次都会读取，不管 shell 是登录式, 交互式或者其它任何类型。 推荐在这里设置环境变量。
> 
> 各配置文件加载顺序：
> 
> `.zshenv` → `.zprofile` → `.zshrc` → `.zlogin` → `.zlogout`
> 

> [!note] 相关资料
> * [.zprofile, .zshrc和.zshenv之间的区别 - 掘金](https://juejin.cn/post/7128574050406367269)
> * [zsh 配置文件解析及优先级](https://einverne.github.io/post/2023/01/zprofile-zshrc.html)
> * [（Manjaro）zsh终端和bash共存时的环境变量配置 - fay小站](http://www.laoluoli.cn/2022/01/10/%ef%bc%88manjaro%ef%bc%89zsh%e7%bb%88%e7%ab%af%e5%92%8cbash%e5%85%b1%e5%ad%98%e6%97%b6%e7%9a%84%e7%8e%af%e5%a2%83%e5%8f%98%e9%87%8f%e9%85%8d%e7%bd%ae/)
>   

#### 历史纪录

```shell
# 指定历史纪录文件
export HISTFILE=~/.zhistory
# 设置历史纪录的最大值
export HISTSIZE=10000
# 注销后保存的历史纪录量大值
export SAVEHIST=10000
# 附加方式写入历史纪录
setopt INC_APPEND_HISTORY
# 如果连续输入相同命令，历史纪录只保留一个
setopt HIST_IGNORE_DUPS
# 这与上面不同的是，如果新命令与旧命令相同，先删除旧的再加新的
setopt HIST_IGNORE_ALL_DUPS
# 同样也是新旧命令相同只留一个
setopt HIST_SAVE_NO_DUPS
# 相同历史路径只保留一个
# setopt PUSHD_IGNORE_DUPS
# 为历史纪录加时间戳
# 选项INC_APPEND_HISTORY，INC_APPEND_HISTORY_TIME和SHARE_HISTORY中只有一个处于活动状态
# setopt INC_APPEND_HISTORY_TIME
```

因 zsh 版本缘故，可能有些历史纪录的选项是用不了的，具体能用哪个，可以使用 `setopt` 和 `unsetopt` 查看。

> [!note] 
> 
> `setopt` 和 `unsetopt` 中的选项写法与 [zsh: 16 Options](https://zsh.sourceforge.io/Doc/Release/Options.html#History) 文档有些出入，比如文档中写的是 `PATH_DIRS`，但命令查询到的是 `pathdirs`，而且这两查询命令好像也不完全。就算 `unsetopt` 查到某选项，也未必真能用。所以还是 `source .zshrc` 时，看报不报错才能确定哪些选项可用。
> 
>> [!failure] 报错
>> 
>>  `command not found: setopt INC_APPEND_HISTORY_TIME`，诸如此类，就是选项不存在的表现，即便使用 `setopt` 或 `unsetopt` 能查到它。
>
>> [!info] 相关链接
>> 
>> * [Zsh/Guide - Gentoo Wiki](https://wiki.gentoo.org/wiki/Zsh/Guide#History)
>> * [unlimited-history-in-zsh](https://qastack.cn/unix/273861/unlimited-history-in-zsh)

### <span id="zsh_conf_insensitivity">忽略大小写</span>

```zsh
zstyle ':completion:*' matcher-list 'm:{a-zA-Z}={A-Za-z}'
```

### 相关资料

* [从 0 开始：教你如何配置 zsh - 知乎](https://zhuanlan.zhihu.com/p/658811059)
* [(zsh) macOS系统环境变量文件 - 简书](https://www.jianshu.com/p/6340c22014b8)
* [Site Unreachable](https://www.cnblogs.com/ma6174/archive/2012/05/08/2490921.html)
* [zshdocs-zh/content/05-files.md· GitHub](https://github.com/ShadowRZ/zshdocs-zh/blob/master/content/05-files.md)

## <span id="zsh_plugins">插件</span>

---

### <span id="zsh_plugins_mgs">插件管理器</span>

#### <span id="zsh_plugins_mgs_ohmyzsh">oh-my-zsh</span>

[oh-my-zsh](https://github.com/ohmyzsh) 是一个强大的 zsh 插件管理器。

oh-my-zsh 因为起步早，所以生态非常完善，缺点就是有点笨重。

##### 插件

oh-my-zsh 的插件非常丰富。

###### git plugin

这个插件主要是给 [Git_Note](../../Git/Git_Note.md) 的一些常用命令起了些「别名」（alias）及一些小功能函数。

有可能这些别名会出现冲突。如这插件将 `git` 命令别名为 `g`，这就与 [GoLang](../../GoLang/GoLang_Note.md) 的一个款多版本管理工具 [g](../../GoLang/GoLang_Note.md#g) 发生冲突。

> [!tip] 
> 
> 解决方法就是，要么不用这个插件，要么就到插件目录中找到相应文件，把那句「别名」代码注释掉就好了。
> 
> oh-my-zsh 的 git 插件文件就在：`.local/share/zinit/snippets/OMZ::plugins--git/git.plugin.zsh/git.plugin.zsh`，把其中 `alias g='git'` 这行代码注释掉就行了，然后 `source .zshrc` 重新让 zsh 生效就可以了。

#### <span id="zsh_plugins_mgs_zinit">zinit</span>

[zinit](https://github.com/zdharma-continuum/zinit) 是一个轻量级的 zsh 插件管理器，速度快。

![zinit compare shotcut](https://raw.githubusercontent.com/zdharma-continuum/zinit/images/startup-times.png)

##### <span id="zsh_plugins_mgs_zinit_install">安装 zinit</span>

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

zinit [自升级](#^3ddfcd)：

```shell
zinit self-update
```

> [!tip] 
> 
> `raw.githubbuserconten.com` 如果下载不了，可以使用 [下载加速网站](../../Git/Git_Note.md#下载加速) 对其加速下载。
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

###### 查看 zinit 的帮助

```shell
zinit help
```

###### 更新

自升级： ^3ddfcd

```shell
zinit self-update
```

更新所有：

```shell
zinit update --all
```

###### 删除

删除所有已安装的插件、snippet（慎用）：

```shell
zinit delete --all
```

清理所有未用到的包：

```shell
zinit delete --clean
```

---

##### <span id="zsh_plugins_mgs_zinit_grammar">zinit 语法</span>

###### load

`load` 命令用于加载并追踪插件

###### light

`light` 命令是更快的加载插件。一般跟着 [GitHub ](../../Git/Git_Note.md#git_github) 的「全限定」库名（格式：账号/库名）。

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

1. 在 `.zshrc` 文件中添加 [忽略大小写代码](Zsh_Note.md#zsh_conf_insensitivity)。
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

[zsh-plugin-condaenv](https://github.com/saravanabalagi/zsh-plugin-condaenv) 是为 zsh 的 [主题](#主题) 提供 [conda](../../Python/Python_Note.md#python_conda) 环境信息。

#### <span id="zsh_plugins_vimod">vi-mod</span>

[zsh-vi-mode](https://github.com/jeffreytse/zsh-vi-mode) 是一个让命令支持 [vim](../../vim/Vim_Note.md) 方式操作的插件。

![vi-mod screenshot](https://user-images.githubusercontent.com/9413601/105746868-f3734a00-5f7a-11eb-8db5-22fcf50a171b.gif)

操作方式与 vim 极其相似。

使用 `ESC` 或 `Ctrl-[` 进入 [普通模式](../../vim/Vim_Note.md#vim_mode_normal)

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

vi-mod 这看插件更「骚」的，竟然还有简单地实现的 vim 的著名插件 [Surround](../../vim/Vim_Plugin.md#Surround) 的小部分功能。

具体查看官方文档：[zsh-vi-mode#Surround](https://github.com/jeffreytse/zsh-vi-mode#Surround)

### <span id="zsh_themes">主题</span>

#### Roundy

[nullxception/roundy](https://github.com/nullxception/roundy)

![roundy preview](https://github.com/nullxception/roundy/raw/main/preview.png)

#### modesty

[modesty](https://github.com/saravanabalagi/zsh-theme-modesty) 这个主题可以显示 [conda 的env](../../Python/Python_Note.md#python_conda_commands_env) 信息。不过得配合安装 [zsh-plugin-condaenv](#zsh-plugin-condaenv) 插件。

![modesty screencast](https://github.com/saravanabalagi/zsh-theme-modesty/raw/master/screencast.gif)

#### alien

[alien](https://github.com/eendroroy/alien) 颜值挺不错的主题。

这个主题还有多种配色可选择。

![alien screenshot](https://camo.githubusercontent.com/cd486bd4cda5af5003161a3924a50625dc7ee1bb20b5be2da4ef58f9cdf64d25/687474703a2f2f61736369696e656d612e6f72672f612f3233373131382e737667)

#### myys

[myys](https://github.com/zhiweichen0012/myys.zsh-theme) 这个主题是 [oh-my-zsh](#oh-my-zsh) 经典主题**ys**的修改款。主要是增加了显示 [conda](../../Python/Python_Note.md#python_conda) 环境。

因为这个库有两个 `theme` 文件，所以得按需求选择一个 theme 来加载，配置如下：

```shell
zinit ice pick"myys.zsh-theme"
zinit light zhiweichen0012/myys.zsh-theme
```

另外，还要在 `.condarc` 文件中增加一句代码：`changeps1: false`，用于 [关闭显示环境名称](../../Python/Python_Note.md#^4d4740)。

> [!info] 
> 
> 这代码是隐藏默认情况会在 prompt 上方显示当前 conda 环境，因为这个修改后的 ys 主题已经将 conda 环境显示「整合」进 prompt 中，所以这默认显示就可以关闭了。

类似的主题还有 [taw-ys](https://github.com/lyytaw/taw-ys.zsh-theme) 这个主题。

##### 问题

###### async git register

还有这类似显示 conda 环境主题，都存在一个 `_defer_async_git_register:4: command not found: _omz_register_handler`。

[GitHub](../../Git/Git_Note.md#git_github) 的 `issue` 上虽然有所谓的解决方法，就是作这个设置：`zstyle ':omz:alpha:lib:git' async-prompt no`，但实际是无效的。

个人解决方案，就是到主题文件中，找到 `git_prompt_info`，将其替换为 `_omz_git_prompt_info`：

```shell
# replace this line
BULLETTRAIN_GIT_PROMPT_CMD="\$(git_prompt_info)"
# to 
BULLETTRAIN_GIT_PROMPT_CMD="\$(_omz_git_prompt_info)"
```

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

zinit 使用 [oh-my-zsh](#zsh_plugins_mgs_ohmyzsh) 的插件，实际目录是放在 `.local/share/zinit/snippets` 这个目录中，各插件都是以 `OMZ::plugins` 开头：

```shell
$ ll .local/share/zinit/snippets 
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2024-04-16 19:34 .
drwx---r-x     - silascript silascript 2024-02-22 23:21 ..
drwxr-xr-x     - silascript silascript 2024-04-17 02:56 OMZ::lib
drwxr-xr-x     - silascript silascript 2024-04-16 19:34 OMZ::plugins--git
drwxr-xr-x     - silascript silascript 2025-08-10 00:58 OMZL::git.zsh

```

#### 相关链接

* [awesome-zsh-plugins](https://github.com/unixorn/awesome-zsh-plugins)
> [!info] 
> 
> 就是个 zsh 的插件、主题集合库。
* [zsh-quickstart-kit](https://github.com/unixorn/zsh-quickstart-kit)

---

### <span>snippet</span>

#### 各种 snippet 整理

##### docker snippet

[docker completion snippet](https://github.com/docker/cli/blob/master/contrib/completion/zsh/_docker)

```shell
zinit ice as"completion" 
zinit snippet https://github.com/docker/cli/blob/master/contrib/completion/zsh/_docker
```

---

## 相关链接

* [ZSH 插件管理器 zinit 介绍及使用](https://www.mivm.cn/zsh-zinit/)
* [不用上 Github 也能用 zinit 管理 zsh 插件](https://www.bilibili.com/read/cv12765752/)
* [我的终端环境：推荐 6 个提升效率的 zsh 插件 - 知乎](https://zhuanlan.zhihu.com/p/663838129)
* [ZSH 系列教程 - Aloxaf's Blog](https://www.aloxaf.com/2020/11/zsh_tutorial_introduce/)
* [zsh 中文文档](https://shadowrz.github.io/zshdocs-zh/)
* [Zsh 开发指南](https://github.com/goreliu/zshguide)
* [ZSH - Documentation](https://zsh.sourceforge.io/Doc/)

---

## 其他笔记

* [Zsh 资料清单](Zsh_Material.md)
* [Linux 笔记](Linux_Note.md)
* [Shell 笔记](Shell/Shell_Note.md)
* [Shell 示例笔记](Shell/Shell_Example.md)
