---
aliases: []
tags:
  - siyuan
  - notesoft
created: 2024-06-24 01:28:18
modified: 2024-06-24 12:00:02
---

# 思源软件笔记

## 目录

---

## 简介

## 安装设置

### 相关目录结构

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
.DS_Store
```
3. 同步
```shell
git remote add origin xxxx.git
git push -u origin "master"
```

> [!info] 参考资料
> 
> * [利用Git同步思源笔记 - SimonLiang20 - 博客园](https://www.cnblogs.com/liangshaoming/p/16974724.html)

---