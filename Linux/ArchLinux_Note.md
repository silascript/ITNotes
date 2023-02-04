---
aliases:
  - 
tags:
  - linux
  - archlinux
  - ibus
  - rime
  - ime
date created: 2022-11-08 11:32
---

# ArchLinux 笔记

---

## 目录

* [配置](#archlinux_configs)
	* [换源](#archlinux_configs_chsources)
* [输入法](#archlinx_ime)
	* [ibus-rime](#ibus-rime)

---

## <span id="archlinux_configs">配置</span>

### <span id="archlinux_configs_chsources">换源</span>

---

## <span id="archlinux_ime">输入法</span>

### <span id="archlinux_ime_ibus_rime">ibus-rime</span>

#### 安装与配置

##### 安装

```shell
# rime ibus版本，所以先得把 ibus框架先装上
sudo pacman -S ibus
yay -S ibus-qt
sudo pacman -S ibus-rime
```

##### 配置

在用户根目录下的 `.profile` 或 `.xprofile` 中添加以下配置：

```
export GTK_IM_MODULE=ibus
export XMODIFIERS=@im=ibus
export QT_IM_MODULE=ibus
ibus-daemon -d -x
```

重启电脑。

这个 `.xprofile` 文件非常重要，如果不配置，极有可能在大部分软件中用不了 rime--wps 除外，哈哈很神奇吧！

> [!tip] 
> arch wiki 建议是在 `.bashrc` 中加入以下配置
> ```
> export GTK_IM_MODULE=ibus
> export XMODIFIERS=@im=ibus
> export QT_IM_MODULE=ibus
>```
> 不过我没配，rime 也能用

##### 相关链接

* [简书 rime 相关](https://www.jianshu.com/p/bf6fa0bdc17f/)
* [ibus arch wiki](https://wiki.archlinuxcn.org/wiki/IBus?rdfrom=https%3A%2F%2Fwiki.archlinux.org%2Findex.php%3Ftitle%3DIBus_%28%25E7%25AE%2580%25E4%25BD%2593%25E4%25B8%25AD%25E6%2596%2587%29%26redirect%3Dno#GNOME)

---
