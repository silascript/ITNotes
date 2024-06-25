---
aliases: []
tags:
  - notesoft
  - logseq
created: 2024-01-25 00:24:07
modified: 2024-06-26 03:37:13
---
# Logseq 笔记

---

## 目录

* [设置](#设置)
* [相关资料](#相关资料)
	* [视频教程](#视频教程)
	* [文字教程](#文字教程)
	* [其他资料](#其他资料)

---

## 相关概念

### 图谱

Logseq 的「图谱」就相当于 [Obsidian](../Obsidian/Obsidian_Note.md) 的 [vault](../Obsidian/Obsidian_Note.md#vault)、[SiYuan](../Siyuan/SiYuan_Note.md) 的 [工作空间](../Siyuan/SiYuan_Note.md#工作空间)。

---

## 设置 

### 相关目录

以 [manjaro](../../Linux/ArchLinux_Note.md#Arch%20各种变体) 系统为例，logseq 主要有两个与配置相关的目录需要关注：

* `.config/Logseq`：
* `~/.logseq`：其中有 `plugins` 子目录用来存放 logseq 插件的。

### Graph 目录

可以选择任意本地目录为新建 Graph 的根目录。这个新「笔记」，在 Logseq 中被称为**Graph**（图谱），类似于 [Obsidian](../Obsidian/Obsidian_Note.md) 中的 [vault](../Obsidian/Obsidian_Note.md#vault)。

Graph 的目录下有三个子目录：

* `journals`：存放每天的日记 [Markdown](../../Markdown/Markdown_Note.md) 文件。
* `logseq`：这是 Graph 级别的配置目录。`.css` 那是主题样式的配置，`edn` 就是快捷键等配置。
* `pages`：存放的是笔记文件，也都是 [Markdown](../../Markdown/Markdown_Note.md) 文件。

---

## 同步

Logseq 已经内置 git 功能，只要在「设置」中开启「git 自动 commit」功能，重启 Logseq，它就会自动 git 初始化。
> [!tip] 
> 
> 可以到你指定「图谱」中看下，开启 git 功能后，会生成 [`.git`文件](../../Git/Git_Note.md#Git%20目录结构)。-- Logseq 比较特殊，不像通用常用的，git 初始化，后生成 `.git` 目录，它用的是 [子目录](../../Git/Git_Note.md#子目录) 的方式。
> 
>> [!info] 
>> 
>> 这个 `.git` 文件是存了一个地址，是指向真正 `.git` 目录的地址，一般是在 `.logseq/git/xxxx/.git`。也就是说，logseq 的 git 的 [仓库](../../Git/Git_Note.md#仓库)，即 「`.git` 目录」，实际是存在 `~/.logseq/git` 目录下。
>>
> `~/.logseq/git` 目录下，存放各个 [图谱](#图谱) 的 git 仓库目录，它们用图谱的路径为目录名，以区隔不同的图谱。
> 
> 具体的目录名命名规则：将 [图谱](#图谱) 路径中的斜杠 `/` 替换成下划线 `_`。如图谱的路径是：`/home/用户名/MyNotes/Logseq_Notes/MyLogs`，而对应的 git 的仓库的实际父级路径为：`~/.logseq/git/_home_用户名_MyNotes_Logseq_Notes_MyLogs`。
> 
>>[!example] 
>>
>> 有一个图谱放在：`~/MyNotes/Logseq_Notes/MyLogs` 目录，它开启 git 功能后，实际 git 仓库目录是放在 `/home/用户名/.logseq/git/_home_用户名_MyNotes_Logseq_Notes_MyLogs`。它的目录结构如下：
>>
>>```shell
>> tree -a ~/.logseq/git
>> /home/用户名/.logseq/git
>>└── _home_用户名_MyNotes_Logseq_Notes_MyLogs
>>  └── .git
>
> ```

### 参考资料

* [使用 Github 作为 Logseq 的数据同步 - 白宦成](https://www.ixiqin.com/2023/01/20/use-a-lot-as-logseq-data-synchronization/)

---

## 相关资料 

### 视频教程

* [Logseq_Videos](Logseq_Videos.md)

### 文字教程

* [Logseq小白系列教程入门篇一 - 知乎](https://zhuanlan.zhihu.com/p/343854552)
* [使用 Github 作为 Logseq 的数据同步 - 白宦成](https://www.ixiqin.com/2023/01/20/use-a-lot-as-logseq-data-synchronization/)

### 其他资料

* [Obsidian和Logseq对比 - 拾月](https://www.skyue.com/22040623.html)
* [Logseq 有哪些好用的插件？ - 知乎](https://www.zhihu.com/question/556039903/answers/updated)
* [笔记软件 Logseq 使用教程 & 学习资源汇总 - 知乎](https://zhuanlan.zhihu.com/p/619887710)

