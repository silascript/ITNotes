---
aliases: []
tags:
  - database
  - mysql
  - oracle
  - sqlserver
  - postgresql
  - db2
  - redis
  - sybase
  - navicat
  - dbeaver
created: 2023-08-18 19:44:52
modified: 2024-07-20 03:29:09
---

# 数据库笔记

---

## 目录

* [MySQL](#MySQL)
* [Oracle](#Oracle)
* [SQLSERVER](#SQLSERVER)
* [POSTgreSQL](#POSTgreSQL)
* [数据库客户端](#database_client)
	* [NaviCat](#NaviCat)
	* [DBeaver](#DBeaver)

---

## MySQL

[MySQL_Note](mysql/MySQL_Note.md)

---

## Oracle

---

## SQLSERVER

---

## POSTgreSQL

--- 

## <span id="database_client">数据库客户端</span>

### NaviCat

---

### DBeaver

[DBeaver](https://dbeaver.io/) [![DBeaver Repo](https://img.shields.io/github/stars/dbeaver/dbeaver?style=social)](https://github.com/dbeaver/dbeaver) 是一个强大的数据库客户端。它比收费的 [NaviCat](#NaviCat) 强大多了。

它支持 MySQL, PostgreSQL, SQLite, Oracle, DB2, SQL Server 等主流数据库。

它有分「社区版」（Community）和「Pro 专业版」。免费的社区版完全够用了。

DBeaver 除了独立安装版还有 [Eclipse 插件版](Java_Note.md#DBeaver) 。

#### 驱动设置及换源

##### 驱动设置

###### 本地文件夹
「本地文件夹」是用来设置驱动保存的目标目录。

设置方式：

设置驱动存储目录：`首选项` -> `连接` -> `驱动`，其中「本地文件夹」就是驱动保存的目录。

DBeaver 使用 [Maven_Note](../Java/Maven/Maven_Note.md) 下载驱动。要知道 maven 国内下载速度可能慢，所以最好添加国内的源。

阿里公共库：[https://maven.aliyun.com/repository/public](https://maven.aliyun.com/repository/public)

「首选项」-「连接」-「驱动」-「Maven」，点「添加」，输入地址，并将新建的源项移到最顶。

> [!info]
> 
> 不同源下载的驱动，不会共同同文件。因为在 [本地文件夹](#本地文件夹) 下按不同源分别存储。
>
> ![DBeaver driver 1](./DataBase_Note.assets/DBeaver_driver_d_1.png)
> 
> 如上图中，「本地文件夹」设置的路径下，`maven` 下有两目录，一个是 maven 的 [中央仓库](../Java/Maven/Maven_Note.md#中央仓库)，而另一个是自行添加的阿里的源。

#### DBeaver 资料

* [DBeaver 显示 系统数据库 方法 - 中国DBA社区](https://www.cndba.cn/dave/article/131425)

---

## 相关笔记

* [SQL 视频清单](SQL_Videos.md)
* [MySQL_Note](mysql/MySQL_Note.md)
* [MySQL常用操作](mysql/MySQL常用操作.md)

