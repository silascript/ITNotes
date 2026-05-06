---
aliases: []
tags:
  - notesoft
  - obsidian
  - cli
  - ai
created: 2026-05-06 21:28:55
modified: 2026-05-06 21:38:52
---

# Obsidian CLI 笔记

---

## 目录

* [目录](#目录)
* [CLI 常用用法](#CLI%20常用用法)

---

## 简介

[Obsidian](Obsidian_Note.md) 从**1.12**版本开始，Obsidian 加入了一个重要功能就是 CLI。

> [!info] 
> 
> 唯一吐槽的是，这个 CLI 是个残疾的 CLI。必须得启动 Obsidan 才能用，也就是说这个 CLI 是得依赖 Obsidian 图形界面的，这设计太蠢了。

---

## CLI 常用用法

### vault 相关

1. 显示 [vault](#vault) 信息

`obsidian vaults` 

如果要显示 [vault](#vault) 的路径，可以加上子参数 `verbose`：

```shell
obsidian vaults verbose
```

效果：

```shell
ITNotes	/home/silascript/MyNotes/ITNotes
WritingNotes	/home/silascript/MyNotes/WritingNotes
LHP_Note	/home/silascript/MyNotes/LHP_Note
WritingExericse	/home/silascript/MyNotes/WritingExericse
```

2. 打开某个 [vault](#vault)

```shell
obsidian vault=vault名称
```

### 插件

`obsidian plugin`

#### 查看插件

1. 查看当前 [vault](#vault) 所有的插件

显示所有插件，无论是否启用：

```shell
obsidian plugins
```

2. 显示已启用的插件：

```shell
obsidian plugins:enabled
```

3. 显示插件版本信息：

* `obsidian plugins versions`
* `obsidian plugins:enabled versions`

4. 查看某插件

需要指定插件的 `id`：

```shell
obsidian plugin id=file-explorer-note-count
```

效果：

```shell
$ obsidian plugin id=file-explorer-note-count
type	community
name	File Explorer Note Count
version	1.2.4
author	Ozan Tellioglu
enabled	true
description	The plugin helps you to see the number of notes under each folder within the file explorer.
```

#### 启用和禁用插件

1. 启用插件：

```shell
obsidan plugin:enable id=插件id
```

2. 禁用插件：

```shell
obsidian plugin:disable id=插件id
```

3. 安装插件

```shell
obsidian plugin:install id=插件
```

> [!tip] 
> 
> `plugin:install` 子命令还有一个属性：`enable`，即安装完后启用该插件。

4. 卸载插件：

```shell
obsidian plugin:uninstall id=插件id
```

5. 重载插件：

```shell
obsidian plugin:reload id=插件id
```

#### 安全模式

查看当前 [安全模式](#安全模式) 状态：

```shell
obsidian plugins:restrict
```

> [!info] 
> 
> * `off`：关闭 [安全模式](#安全模式)，能够浏览、安装和使用插件社区中 [第三方插件](#obn_plugins_commp)
> * `on`：开启 [安全模式](#安全模式)，[第三方插件](#obn_plugins_commp) 不能安装、使用等操作

关闭 [安全模式](#安全模式)：

```shell
obsidian plugins:restrict off
```

开启 [安全模式](#安全模式)：

```shell
obsidian plugins:restrict on
```

---

## 相关笔记

* [Obsidian 笔记](Obsidian_Note.md)
* [Obsidian 资料](Obsidian_Material.md)
* [Obsidian 视频清单](Obsidian_Videos.md)
