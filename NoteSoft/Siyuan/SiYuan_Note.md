---
aliases: []
tags:
  - siyuan
  - notesoft
created: 2024-06-24 01:28:18
modified: 2024-07-06 23:17:56
---

# 思源笔记软件笔记

---

## 目录

* [相关目录结构](#相关目录结构)
	* [配置目录](#配置目录)
	* [工作空间](#工作空间)

---

## 简介

思源笔记是一个魔改 [Markdown](../../Markdown/Markdown_Note.md) 的笔记软件。

---

## 安装设置

### 安装

### 相关目录结构

#### 配置目录

`.config` 目录下有三个目录：

* `siyuan`
* `SiYuan`
* `SiYuan-Electron`

这三个目录删除，启动思源笔记软件，会自动重新创建。

#### 工作空间

思源笔记的「工作空间」等同于 [Obsidian](../Obsidian/Obsidian_Note.md) 的 [vault](../Obsidian/Obsidian_Note.md#vault)。

工作空间目录结构：

* `conf`：工作空间的配置目录，就是「设置」页面中的配置信息存放目录。当指定一个目录为工作空间时，会自动生成此目录。
* [data](#data%20目录)：数据目录，最重要的目录，笔记都放在这个目录。
* `history`：历史记录目录。
* `repo`：数据快照目录。
* `temp`：临时文件目录，主要是索引、升级安装文件之类。

##### data

工作空间下的 `data` 目录，就是用户的数据目录，[同步](#同步) 的核心目录。

`data` 目录结构：

* `.siyuan`：只有一个 syncignore 文件。
* `asserts`：附件目录，插入到笔记的图片、PDF 等附件
* `emojis`：自定义 emojis 表情包
* `snippets`：代码片段目录，「设置」-「外观」-「代码片段」的数据就保存在这里
* `templates`：模板目录。「设置」-「[集市](#集市)」-「[模板](#模板)」中下载的模板就保存在这里
* `widgets`：挂件目录，「设置」-「[集市](#集市)」-「[挂件](#挂件)」就保存在这里

###### storage

`storage` 目录下，[petal](https://github.com/siyuan-note/petal) 目录，是插件接口目录。

#### 默认工作空间

打开思源笔记软件，至少存在一个工作空间，如果 [安装](#安装) 完，第一次启动，它会让你指定一个工作空间，这个工作空间，可以认为就是「默认工作空间」。这空间主要就是用来存放放用户指南的。

在用户根目录下，`SiYuan` 目录是默认工作空间，如果删除，启动思源笔记软件后，会自动重新创建。

> [!info] 相关资料
> 
> * [siyuan dock - 思源是如何存储数据的](https://github.com/siyuan-note/siyuan/blob/master/README_zh_CN.md#%E6%80%9D%E6%BA%90%E6%98%AF%E5%A6%82%E4%BD%95%E5%AD%98%E5%82%A8%E6%95%B0%E6%8D%AE%E7%9A%84)

---

## 同步

使用 [Git_Note](../../Git/Git_Note.md) 及 [github](https://github.com) 来同步思源笔记的数据。

实际就是使用 git 管理和同步思源的 [工作空间](#工作空间)。

大致步骤：

1. 进入 [工作空间](#工作空间)，初始化 `git init`
2. 添加 [gitignore](../../Git/Git_Note.md#gitignore) 文件（同步 `data` 目录）
```gitignore
/backup
/conf
/history
/sync
/temp
/corrupted
/data/widgets/
/data/plugins/
/data/storage/petal/
/data/20210808180117-czj9bvb/
.DS_Store
```
> [!tip] 
> 
> `/data/20210808180117-czj9bvb/` 这个是用户指南，每次新建或打开一个 [工作空间](#工作空间) 都会加入这货，所以得把这货添加到忽略文件中。
3. 同步
```shell
git remote add origin xxxx.git
git push -u origin "master"
```

> [!info] 参考资料
> 
> * [利用Git同步思源笔记 - SimonLiang20 - 博客园](https://www.cnblogs.com/liangshaoming/p/16974724.html)

---

## 特色

### markdown

思源笔记支持 [Markdown](../../Markdown/Markdown_Note.md) 语法，并且是「**所见即所得**」。

### 双向链接

思源笔记支持文档间的「双向链接」，跟 [Obsidian](../Obsidian/Obsidian_Note.md) 一样。

### 自动保存

思源笔记是自动保存写作内容的，无需手动保存。

### 跨平台支持

思源笔记支持 Windows、Linux 和 Mac 系统。

---

## 核心概念

### 块

「块」是思源笔记最重要的概念，它是思源笔记**最小单位**。

> [!important]
> 
> 思源笔记默认以块为单位来保存文档内容。

![块种类](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/840e48ae24134333900f5f630c40d9d7~tplv-k3u1fbpfcp-jj-mark:3024:0:0:0:q75.awebp#?w=330&h=836&s=56697&e=png&b=f9f9f9)

[叶子块](#叶子块) 和 [容器块](#容器块) 的设计，估计是源于 [行内元素](../../Frontend/Html_Note.md#行内元素) 和 [块级元素](../../Frontend/Html_Note.md#块级元素)。

#### 叶子块

#### 容器块

#### 文档块

#### 内容块

### Markdown

思源也支持 [Markdown](../../Markdown/Markdown_Note.md) 语法。

### 标签

标签，Tag，与 [Obsidian](../Obsidian/Obsidian_Note.md) 一样的东西。

---

## 快捷功能

### 优化排版

「优化排版」这个小功能，能快速对中英文混全排版进行优化。
> [!tip] 
> 
> 其实就是中英文间加空格什么的。

### 常用快捷键

* `/`：可以弹出命令，输入各种块、新建文档。
> [!tip] 
> 
> 类似 [Obsidian](../Obsidian/Obsidian_Note.md) 中 `Ctrl+p`[命令面板快捷键](../Obsidian/Obsidian_Note.md#obn_default_hotkeys)。
* `Ctrl+a`：选取块
* `Ctrl+a` 两次：当前文档全选
* `[[` 或 `((`：建立链接。按下 `[[` 或 `((` 后，会触发 [块](#块) 的搜索，输入关键字来定位 [内容块](#内容块) 和 [文档块](#文档块)，从而建议链接。这与 [Obsidian](../Obsidian/Obsidian_Note.md) 建立链接的操作类似。
* `Ctrl+p`：全局搜索

---

## 集市

### 插件

### 主题

### 挂件

### 模板

---

## 相关资料

* [思源笔记快速上手指南 - 掘金](https://juejin.cn/post/7309755128852365364)
* [思源笔记·学习资源](https://flowus.cn/hub001/share/7f64c804-79c1-443f-929d-240066953b41)
* [基于思源笔记的数据库使用分享 - 少数派](https://sspai.com/post/87423)
* [思源笔记使用心得 - 哔哩哔哩](https://www.bilibili.com/read/cv21936410/)
* [【Achuan-2】思源笔记教程和心得分享](https://www.yuque.com/achuan-2/siyuan)

---

## 相关笔记

* [思源笔记视频清单](SiYuan_Videos.md)
* [笔记软件笔记](../NoteSoft_Note.md)
* [Logseq笔记](../Logseq/Logseq_Note.md)
* [Obsidian笔记](../Obsidian/Obsidian_Note.md)
* [AnyType笔记](../AnyType/AnyType_Note.md)

