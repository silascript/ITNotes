---
aliases: []
tags:
  - linux
  - ranger
  - joshuto
created: 2023-08-18 19:44:52
modified: 2024-02-19 03:35:23
---
# Ranger 相关

---

## 目录

* [Ranger简介](#ranger_introduction)
* [安装及基础配置](#ranger_install_settings)
	* [安装](#安装)
	* [配置](#配置)
* [快捷键](#ranger_hotkey)
* [Joshuto](#Joshuto)
* [yazi](#yazi)

---

## <span id="ranger_introduction">Ranger 简介</span>

Ranger Github 地址：[https://github.com/ranger/ranger](https://github.com/ranger/ranger)

## <span id="ranger_install_settings">安装及基础配置</span>

### 安装

```shell
sudo pacman -S ranger
```

### 配置

#### 生成配置文件

ranger 配置文件是放在 `~/.config/ranger/` 这个目录下的，新安装的 ranger 是没有配置文件，所以得生成默认的配置文件：

```shell
# 在~/.config/ranger目录下生成一堆配置文件。主要配的是scope.sh和rc.conf
ranger --copy-config=all
```

#### 安装图标

```shell

# 安装图标
git clone https://github.com/alexanderjeurissen/ranger_devicons ~/.config/ranger/plugins/ranger_devicons

```

```shell
echo "default_linemode devicons" >> $HOME/.config/ranger/rc.conf
```

> [!tip] RANGER_LOAD_DEFAULT_RC
> 如果要使用 `~/.config/ranger` 目录下的配置生效，需要把 `RANGER_LOAD_DEFAULT_RC` 变量设置为 `false`：
>
> ```shell
>
> # bash
> echo export RANGER_LOAD_DEFAULT_RC=FALSE >> ~/.bashrc
>
> # zsh
> echo "export RANGER_LOAD_DEFAULT_RC=false">>~/.zshrc
> 
> ```

> [!tip]
> 其实如果 bash 或 zsh 使用了 [lsd](Linux_Note.md#lsd) 或 [exa](Linux_Note.md#exa)，那就会自带图标，不用在 ranger 里另设了。

#### 预览

##### 图片预览

使用**w3m**或 [ueberzug](https://github.com/seebye/ueberzug) 。

w3m 是终端 web 浏览器

如果终端使用 w3m 不支持不生效，就使用 [ueberzug](https://github.com/seebye/ueberzug) 。

配置 rc.conf 如下:

```shell
set preview_images true
set preview_images_method ueberzug
```

---

## <span id="ranger_hotkey">快捷键</span>

Ranger 的操作与 [Vim](../vim/Vim_Note.md) 操作相近，用了很多的 vim 的操作习惯。

### 目录跳转

|     快捷键      |                                                   功能                                                    |
|:---------------:|:---------------------------------------------------------------------------------------------------------:|
|        H        |                                                   后退                                                    |
|        L        |                                                   前进                                                    |
|       gg        |                                                 跳到顶端                                                  |
|        G        |                                                 跳到底端                                                  |
|        /        |                                                   搜索                                                    |
|        g        |                                               快速进入目录                                                |
|       gh        |                                              跳回 home 目录                                               |
|       gr        |                                              跳到系统根目录                                               |
|       ge        |                                             跳转 `/etc` 目录                                              |
|        S        |                                       切换到 ranger 最后浏览的目录                                        |
|        q        |                                             退出 ranger 模式                                              |
|       ZZ        |                                                   同上                                                    |
|       F1        |                                                   帮助                                                    |
| l 或 向右方向键 |                                            进入光标所在的目录                                             |
| h 或 向左方向键 |                                               退回上层目录                                                |
| j 或 向上方向键 |                                               向上移动光标                                                |
| k 或 向下方向键 |                                               向下移动光标                                                |
|        r        |                                          显示打开文件的选项菜单                                           |
|       `[`       |                                           父级目录向上移动光标                                            |
|       `]`       |                                           父级目录向下移动光标                                            |
|     空格键      |                                            选择或取消选择文件                                             |
|       dd        |                                                   剪切                                                    |
|       pp        |                                                   粘贴                                                    |
|       yy        |                                                   复制                                                    |
|       dD        |                                                   删除                                                    |

### 标记

|   快捷键    |                                                       功能                                                       |
|:-----------:|:----------------------------------------------------------------------------------------------------------------:|
|      m      | 列出当前已经进行标识的路径。<br> 并准备对当前目录进行标记。<br> 如果再按一个字符，这个字符将作为此路径的标记字符 |
|   m+ 字符   |                                                    标记某路径                                                    |
|    &#96;    |                                    打开标记列表，可以选择或输入相应的标记字符                                    |
| &#96;+ 字符 |                                             同上，跳转标记对应的目录                                             |
|     um      |                                        列出可删除标记，选择指定删除的标记                                        |
|  um+ 字符   |                                                同上，删除指定标记                                                |
|             |                                                                                                                  |
|             |                                                                                                                  |
|             |                                                                                                                  |
|             |                                                                                                                  |
|             |                                                                                                                  |

---

## Ranger 视频清单

* [终端文件管理器(ranger)配置使用](https://www.bilibili.com/video/BV1ER4y1F72A)
* [命令行文件管理神器ranger](https://www.bilibili.com/video/BV1up4y1b7iJ)
* [Ranger终极配置方案--TheCW](https://www.bilibili.com/video/BV1b4411R7ck)

---

## Ranger 相关资料连接

* [ranger配置和使用](https://www.zssnp.top/2021/06/03/ranger/)

---

## Joshuto

[Joshuto](https://github.com/kamiyaa/joshuto) 是一款使用 [Rust](../Rust/Rust_Note.md) 写的「类 Ranger」终端文件管理器。可以认为这货就是 「Rust 版的 [Ranger ](#Ranger%20相关)--「 [ranger](https://github.com/ranger/ranger)-like terminal file manager written in Rust.」

Joshuto 有个文档来对比各家终端文件管理器：[nnn, ranger, lf, joshuto, yazi, which is your choice? · kamiyaa/joshuto · Discussion #454 · GitHub](https://github.com/kamiyaa/joshuto/discussions/454)，不知道选哪个的可以看下参考下。

### 相关资料

[Joshuto 文档](https://github.com/kamiyaa/joshuto/tree/main/docs)

---

## yazi

[yazi](https://github.com/sxyazi/yazi) 跟 [Joshuto](#Joshuto) 一样，是使用 [Rust](../Rust/Rust_Note.md) 编写的终端文件管理器。

yazi 使用 [Ueberzug++](https://github.com/jstkdng/ueberzugpp) 来作图片组件。这货是 [ueberzug](https://github.com/seebye/ueberzug) 的「平替版」，因为 ueberzug 已经不维护了。

### 相关资料

* [5K+ Star！终端命令行版文件管理器 - 知乎](https://zhuanlan.zhihu.com/p/677949849#:~:text=%E8%BF%99%E6%A0%B7%E6%AF%8F%E6%AC%A1%E4%BD%BF%E7%94%A8%E6%97%B6%E5%8F%AA,%E3%80%8D%E5%91%BD%E4%BB%A4%E5%8F%AF%E4%BB%A5%E9%80%80%E5%87%BA%E3%80%82)
* [ Yazi Doc Quick Start](https://yazi-rs.github.io/docs/quick-start)

