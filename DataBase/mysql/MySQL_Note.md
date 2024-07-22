---
aliases: []
tags:
  - database
  - mysql
created: 2023-01-30 11:19:11
modified: 2024-07-22 21:37:34
---

# MySQL 笔记

---

## 目录

* [客户端工具](#mysqln_client_tools)
* [版本](#版本)

---

## <span id="mysqln_client_tools">客户端工具</span>

### mycli

[mycli](https://www.mycli.net) 是一个阿三写的终端增强客户端，比 MySQL 自带的好用多了。
这货是用 Python 写的，所以通过 `pip install mycli` 命令来安装。
mycli 支持 MySQL、MariaDB、Percona。

mycli 有语法高亮、代码提示、分页显示等非常实用的功能。

![mycli columns](https://www.mycli.net/images/columns.png)

配置：
`~/.myclirc` 是配置文件。

常用配置：

`syntax_style` 配置语法配色，值就里配色方案的名称。
> [!info] 
> 
> 官方提供了几个 [配色](https://www.mycli.net/syntax)。

### 连接问题

如果在连接 MySQL 8.x 时，出现 `Public Key Retrieval is not allowed` 错误，请开启相应的权限。 

### 图形界面客户端

推荐免费的 [DBeaver](../DataBase_Note.md#DBeaver)。

---

## 版本

* MySQL Enterprise Edition：企业版，提供企业版备份工具、线程池、防火墙、审计、监控等功能。
* MySQL Cluster 企业版：MySQL Cluster CGE，是一套基于内存、无共享的高勅方案，底层使用的是 NDB 存储引擎。
* MySQL 社区版：免费版，遵循 GPL 协议。

GA：General Availability Releases 正式版。一般情况下，MySQL 的发布 GA 版本之前，会有三个 RC 版本。

GA 有多个下载，除了 32 位及 64 位区别外，最大区别是分 `glibc2.12​` ​和 `glibc2.17`​ ​两版本。

> [!tip] 
> 
> ​`glibc2.12​` 和 `glibc2.17​` 是编译 MySQL 的 glibc 版本。
> 
> 这两个 glibc 区别：`glibc2.17​` ​剔除了 debug 相关的二进制文件及 debug symbol，所以体积比 `glibc2.12​` ​小。

Oracle 又要刷版本号了，弄出个「创新」和「LTS」版本。这两个都是生产级质量。其实就是把包含之前的补丁加新功能的版本重命名，把版本啊升了个级：如本来是 8.0.x 的，这样容易给人好像没怎么升，所以干脆，更名为 8.x，这样看起来有比较大的更新。

未来的计划：8.4.x、9.7.x 是 LTS，其余 8.x 及 9.x 是「创新」版。

![mysql version schedule screenshot](https://developer.qcloudimg.com/http-save/10653659/60a817fc9e60ec8daebd29fe56699ab8.png)

### 相关资料

* [MySQL8.1来了：MySQL创新和长期支持（LTS）版本简介-腾讯云开发者社区-腾讯云](https://cloud.tencent.cn/developer/article/2303772)
* [MySQL 8.4版本发布与历史版本回顾 – orczhou.com](https://www.orczhou.com/index.php/2024/05/mysql-8-4-and-version-history/)
* [如何选择适合的 MySQL Connector/J 版本](https://segmentfault.com/a/1190000044667101)

---

## 相关内容

* [Linux 下安装 MySQL](linux下安装mysql.md)
* [MySQL常用操作](MySQL常用操作.md)
* [Docker 安装 MySQL](../../Docker/Docker_Note.md#dk_softc_demo_mysql)
* [MySQL 视频清单](MySQL_Videos.md)