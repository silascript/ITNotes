---
aliases: []
tags:
  - siyuan
  - notesoft
created: 2024-06-24 01:28:18
modified: 2024-06-29 01:02:31
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

## 核心概念

### 块

「块」是思源笔记最重要的概念，它是思源笔记**最小单位**。

> [!important]
> 
> 思源笔记默认以块为单位来保存文档内容。

### Markdown

思源也支持 [Markdown](../../Markdown/Markdown_Note.md) 语法。

---

## 快捷功能

### 优化排版

「优化排版」这个小功能，能快速对中英文混全排版进行优化。
> [!tip] 
> 
> 其实就是中英文间加空格什么的。

### 常用快捷键

* `Ctrl+a`：选取块
* `Ctrl+a` 两次：当前文档全选

---

## 集市

### 插件

### 主题

### 挂件

### 模板

---

## 相关资料

* [思源笔记快速上手指南 - 掘金](https://juejin.cn/post/7309755128852365364)
* [基于思源笔记的数据库使用分享 - 少数派](https://sspai.com/post/87423)
* [思源笔记使用心得 - 哔哩哔哩](https://www.bilibili.com/read/cv21936410/)

---

## 相关笔记

* [思源笔记视频清单](SiYuan_Videos.md)
* [笔记软件笔记](../NoteSoft_Note.md)
* [Logseq笔记](../Logseq/Logseq_Note.md)
* [Obsidian笔记](../Obsidian/Obsidian_Note.md)
* [AnyType笔记](../AnyType/AnyType_Note.md)

