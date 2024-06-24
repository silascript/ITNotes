---
aliases: []
tags:
  - siyuan
  - notesoft
created: 2024-06-24 01:28:18
modified: 2024-06-24 12:24:39
---

# 思源软件笔记

## 目录

---

## 简介

## 安装设置

### 安装

### 相关目录结构

#### 工作空间

#### 配置目录

`.config` 目录下有三个目录：

* `siyuan`
* `SiYuan`
* `SiYuan-Electron`

这三个目录删除，启动思源笔记软件，会自动重新创建。

#### 默认工作空间

打开思源笔记软件，至少存在一个工作空间，如果 [安装](#安装) 完，第一次启动，它会让你指定一个工作空间，这个工作空间，可以认为就是「默认工作空间」。这空间主要就是用来存放放用户指南的。

在用户根目录下，`SiYuan` 目录是默认工作空间，如果删除，启动思源笔记软件后，会自动重新创建。

> [!info] 相关资料
> 
> * [siyuan dock - 思源是如何存储数据的](https://github.com/siyuan-note/siyuan/blob/master/README_zh_CN.md#%E6%80%9D%E6%BA%90%E6%98%AF%E5%A6%82%E4%BD%95%E5%AD%98%E5%82%A8%E6%95%B0%E6%8D%AE%E7%9A%84)

#### 工作空间

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