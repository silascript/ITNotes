---
aliases: []
tags:
  - db
  - database
  - mysql
  - oracle
  - sqlserver
  - postgresql
  - sqlite
  - db2
  - redis
  - sybase
  - navicat
  - dbeaver
created: 2023-08-18 19:44:52
modified: 2025-04-07 23:30:16
---

# 数据库笔记

---

## 目录

* [数据库原理](#数据库原理)
* [各数据库笔记](#各数据库笔记)
	* [MySQL](#MySQL)
	* [Oracle](#Oracle)
	* [SQLSERVER](#SQLSERVER)
	* [POSTgreSQL](#POSTgreSQL)
	* [SQLite](#SQLite)
* [数据库客户端](#database_client)
	* [NaviCat](#NaviCat)
	* [DBeaver](#DBeaver)

---

## 数据库原理

### 范式

构造数据库必须遵循一定的规则，在关系型数据库中，这种规则就称为「**范式**」。

按规范化程度从低到高可分为 5 级范式。一般来说，数据库只需要东路第三范式。

> [!quote] 
> 
> 规范化的基本思想是逐步消除数据依赖中不合适的部分，使模式中的各关系模式达到栽种程序的「分离」，即「一事一地」的模式设计原则。-- 《数据库系统概论》第 5 版。

#### 第一范式

#### 第二范式

#### 第三范式

---

## 各数据库笔记

### MySQL

[MySQL_Note](mysql/MySQL_Note.md)

### Oracle

### SQLSERVER

### POSTgreSQL

### SQLite

[SQLite 笔记](SQLite/SQLite_Note.md)

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

#### 链接设置

有一些比较常用的连接设置：

* `allowMultiQueries`：允许执行多条查询语句
> [!tip] 
> 
> 这个设置默认是 `false`，所以会出现「执行 SQL 脚本」没错，但选择多条 SQL 语句点「执行 SQL 语句」时，就会出现 `1064` 的错误，提示是语法错误，但事实语法是没错的，就是这个设置禁止了多条 SQL 语句的执行才造成的。

---

## 相关笔记

* [SQL笔记](SQL_Note.md)
* [MySQL笔记](mysql/MySQL_Note.md)
* [数据库资料](mysql/DataBase_Material.md)
* [MySQL资料](mysql/MySQL_Material.md)
* [MySQL常用操作](mysql/MySQL常用操作.md)

