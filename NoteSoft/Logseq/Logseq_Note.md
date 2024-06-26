---
aliases: []
tags:
  - notesoft
  - logseq
created: 2024-01-25 00:24:07
modified: 2024-06-26 19:53:18
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

## 设置 

### 相关目录

以 [manjaro](../../Linux/ArchLinux_Note.md#Arch%20各种变体) 系统为例，logseq 主要有两个与配置相关的目录需要关注：

* `.config/Logseq`：
* `~/.logseq`：其中有 `plugins` 子目录用来存放 logseq 插件的。

### Graph

 #图谱 

可以选择任意本地目录为新建 Graph 的根目录。这个新「笔记」，在 Logseq 中被称为**Graph**（「图谱」）。

Logseq 的「图谱」就相当于 [Obsidian](../Obsidian/Obsidian_Note.md) 的 [vault](../Obsidian/Obsidian_Note.md#vault)、[SiYuan](../Siyuan/SiYuan_Note.md) 的 [工作空间](../Siyuan/SiYuan_Note.md#工作空间)。

Graph 的目录下有三个子目录：

* `journals`：存放每天的日记 [Markdown](../../Markdown/Markdown_Note.md) 文件。
* `logseq`：这是 Graph 级别的配置目录。`.css` 那是主题样式的配置，`edn` 就是快捷键等配置。
* `pages`：存放的是笔记文件，也都是 [Markdown](../../Markdown/Markdown_Note.md) 文件。

---

## 同步

使用 [Git](../../Git/Git_Note.md)+[GitHub](../../Git/Git_Note.md#GitHub%20使用) 同步 Logseq 数据大概在两种方案：

1. 自建 git 仓库，然后同步
2. 开启 Logseq 内置的 git 功能，自动同步

### 自建 git 仓库

### Logseq 的 git 功能

Logseq 已经内置 git 功能，只要在「设置」中开启「git 自动 commit」功能，重启 Logseq，它就会自动 git 初始化。

> [!tip] 
> 
> 可以到你指定「图谱」中看下，开启 git 功能后，会生成 [.git文件](../../Git/Git_Note.md#Git%20目录结构)。-- Logseq 比较特殊，不像通用常用的，git 初始化，后生成 `.git` 目录，它用的是 [子目录](../../Git/Git_Note.md#子目录) 的方式。
> 
>> [!info] 
>> 
>> 这个 `.git` 文件是存了一个地址，是指向真正 `.git` 目录的地址，一般是在 `.logseq/git/xxxx/.git`。也就是说，logseq 的 git 的 [仓库](../../Git/Git_Note.md#仓库)，即 「`.git` 目录」，实际是存在 `~/.logseq/git` 目录下。
>>
> `~/.logseq/git` 目录下，存放各个 [图谱](#Graph) 的 git 仓库目录，它们用图谱的路径为目录名，以区隔不同的图谱。
> 
> 具体的目录名命名规则：将 [图谱](#Graph) 路径中的斜杠 `/` 替换成下划线 `_`。如图谱的路径是：`/home/用户名/MyNotes/Logseq_Notes/MyLogs`，而对应的 git 的仓库的实际父级路径为：`~/.logseq/git/_home_用户名_MyNotes_Logseq_Notes_MyLogs`。
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

Logseq 的 git 功能同步三个目录：

* `journals`
* `logseq`
* `pages`

如果使用 [自建 git 仓库](#自建%20git%20仓库) 同步方案时，也应同步这三个目录，其他文件就把它们加到 [gitignore](../../Git/Git_Note.md#gitignore) 文件中，让 [Git](../../Git/Git_Note.md) 忽略到它们好了。

### 参考资料

* [使用 Github 作为 Logseq 的数据同步 - 白宦成](https://www.ixiqin.com/2023/01/20/use-a-lot-as-logseq-data-synchronization/)
* [GitHub - CharlesChiuGit/Logseq-Git-Sync-101](https://github.com/CharlesChiuGit/Logseq-Git-Sync-101)
* [Logseq 系列之 Git 同步 - samwei12 - 博客园](https://www.cnblogs.com/samwei12/p/logseq-xi-lie-zhi-git-tong-bu.html)
* [Logseq 同步方案设计](https://blog.tomyail.com/how-to-sync-logseq-notes-between-icloud-and-github/)

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

