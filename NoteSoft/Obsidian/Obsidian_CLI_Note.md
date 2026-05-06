---
aliases: []
tags:
  - notesoft
  - obsidian
  - cli
  - ai
created: 2026-05-06 21:28:55
modified: 2026-05-07 05:14:43
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

通过 `obsidian help` 查看 [vault](Obsidian_Note.md#vault) 相关的 API：

```shell
vault                 Show vault info
    info=name|path|files|folders|size  - Return specific info only

vaults                List known vaults
    total               - Return vault count
    verbose             - Include vault paths
```

1. 显示所有 [vault](#vault) 信息

`obsidian vaults` ：

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

2. 打某个 [vault](#vault) 

```shell
obsidian vault=vault名称
```

> [!info] 
> 
> 如果不指定 vault 名称，即 `obsidian vault`，则相当于指定了焦点所在的 vault（因为 Obsidan CLI 要使用必须先启动 Obsidian，所以必会打开一个 vault）, vault 是否处于焦点，可以查看 vault 列表， vault 列表中有个「**✓**」的，就表明这个 vault 处于「焦点」状态，如果这时执行 `obsidian vault` 命令，就会在终端中显示该 vault 的各种信息，示例如下：
> 
> ```shell
> $ obsidian vault        
> name	ITNotes
> path	/home/silascript/MyNotes/ITNotes
> files	433
> folders	121
> size	20680940
> ```
> 
> 而如果明文指定要打开的 vault，而该 vault 是「焦点」状态，则会在终端显示相关的操作菜单。意思就是，当前 vault 已经处于打开并处于「焦点」状态，你可以对其进行下一步的操作。示例：
> 
> ```shell
> obsidian vault=ITNotes
> ```
> 
> ![obsidian cli vault sc](obsidian_cli_vault_sc.gif)
> 

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
