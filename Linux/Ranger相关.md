---
aliases:
  - 
tags:
  - linux
  - ranger
  - joshuto
created: 2022-11-7 2:50:13
modified: 2023-05-29 2:27:40
---
# Ranger 相关

---

## 目录

* [Ranger简介](#ranger_introduction)
* [安装及基础配置](#ranger_install_settings)
* [快捷键](#ranger_hotkey)
* [Joshuto](#Joshuto) 

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
echo export RANGER_LOAD_DEFAULT_RC=FALSE >> ~/.bashrc
```

#### 预览

##### 图片预览

使用**w3m**或**ueberzug**

w3m 是终端 web 浏览器

如果终端使用 w3m 不支持不生效，就使用**ueberzug**

配置 rc.conf 如下:

```shell
set preview_images true
set preview_images_method ueberzug
```

---

## <span id="ranger_hotkey">快捷键</span>

Ranger 的操作与 [Vim](../vim/Vim_Note.md) 操作相近，用了很多的 vim 的操作习惯。

|     快捷键      |             功能             |
|:---------------:|:----------------------------:|
|        H        |             后退             |
|        L        |             前进             |
|       gg        |           跳到顶端           |
|        G        |           跳到底端           |
|        /        |             搜索             |
|        g        |         快速进入目录         |
|        S        | 切换到 ranger 最后浏览的目录 |
|        q        |       退出 ranger 模式       |
|       ZZ        |             同上             |
|       F1        |             帮助             |
| l 或 向右方向键 |      进入光标所在的目录      |
| h 或 向左方向键 |         退回上层目录         |
| j 或 向上方向键 |         向上移动光标         |
| k 或 向下方向键 |         向下移动光标         |
|        r        |    显示打开文件的选项菜单    |
|       `[`       |     父级目录向上移动光标     |
|       `]`       |     父级目录向下移动光标     |
|     空格键      |      选择或取消选择文件      |
|       dd        |             剪切             |
|       pp        |             粘贴             |
|                 |                              |
|                 |                              |
|                 |                              |
|                 |                              |
|                 |                              |
|                 |                              |

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

[Joshuto 文档](https://github.com/kamiyaa/joshuto/tree/main/docs)

