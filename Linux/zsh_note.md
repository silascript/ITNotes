---
aliases:
  - 
tags:
  - 
created: 2022-11-7 2:50:13
modified: 2023-05-1 6:31:08
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

#### <span id="zsh_plugins_mgs_zinit">zinit</span>

##### <span id="zsh_plugins_mgs_zinit_commands">zinit 常用命令</span>

更新所有：
```shell
zinit update --all
```

---

### <span id="zsh_plugins_common">常用插件</span>

#### <span id="zsh_plugins_synhl">fast-syntax-highlighting</span>

~~[fast-syntax-highlighting](https://github.com/zdharma/fast-syntax-highlighting)~~

[新的 fast-syntax-highlighting](https://github.com/zdharma-continuum/fast-syntax-highlighting)

高亮各种命令，为命令着色~
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

 这个插件会搜索你输入的历史记录。

 常用功能
 Ctrl+r: 列出相应的历史记录的菜单列表
 Ctrl+n: 向下移动菜单候选项
 Ctrl+p: 向上移动候选项
 Ctrl+k/Enter: 确认选中候选项并返回结果
 Ctrl+v: 取消搜索

 常用配置:

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

输入命令历史 (浅色) 提示

常用操作:
按方向键 ->可补全。

#### <span id="zsh_plugins_zcompletions">zsh-completions</span>

[zsh-completions](https://github.com/zsh-users/zsh-completions)

[![asciicast](https://asciinema.org/a/266997.svg)](https://asciinema.org/a/266997)

---

## 相关链接

* [ZSH 插件管理器 zinit 介绍及使用](https://www.mivm.cn/zsh-zinit/)
* [不用上 Github 也能用 zinit 管理 zsh 插件](https://www.bilibili.com/read/cv12765752/)
