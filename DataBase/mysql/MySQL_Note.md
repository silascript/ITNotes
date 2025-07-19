---
aliases: []
tags:
  - database
  - db
  - mysql
created: 2023-01-30 11:19:11
modified: 2025-07-19 10:19:04
---

# MySQL 笔记

---

## 目录

* [客户端工具](#mysqln_client_tools)
* [版本](#版本)

---

## <span id="mysqln_client_tools">客户端工具</span>

### mycli

[mycli](https://www.mycli.net) [![mycli repo](https://img.shields.io/github/stars/dbcli/mycli
)](https://github.com/dbcli/mycli) 是一个阿三写的终端增强客户端，比 MySQL 自带的好用多了。
这货是用 Python 写的，所以通过 `pip install mycli` 命令来安装。
mycli 支持 MySQL、MariaDB、Percona。

mycli 有语法高亮、代码提示、分页显示等非常实用的功能。

![mycli columns](https://www.mycli.net/images/columns.png)

#### mycli 配置

`~/.myclirc` 是配置文件。

##### 常用配置项

* `syntax_style`： 配置语法配色，值就里配色方案的名称。
> [!info] 
> 
> manni, igor, xcode, vim, autumn, vs, rrt,
> native, perldoc, borland, tango, emacs, friendly, monokai, paraiso-dark,
> colorful, murphy, bw, pastie, paraiso-light, trac, default, fruity
> 
> 官方提供了几个 [配色](https://www.mycli.net/syntax)。
> 
* `key_bindings`​：快捷键绑定，两种选项：`Emac` 和 `vi`。默认是 `Emac`

#### mycli 使用

mycli 登录与 mysql 自带的客户端几乎一样。

##### 字符集问题

 mycli 默认情况下，存在客户端字符集问题，使用 mycli 登录时，使用 `status` 或 `show variables like "%char%";` 命令查询字符集，`Client characterset` 和 `Conn.characterset` 还是 `utf8mb3`，这即便已经在 mysql 的配置文件中 `[client]` 中设置了 `default-character-set = utf8mb4`，也是无效。
 
不过幸好 [github](https://github.com) 上有解决方法，就是加字符集参数：

```shell
mycli -h localhost -P 3356 -u silascript -p 123456 --charset=utf8mb4
```

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

### LTS

MySQL 进入 8.x 时代，版本就会成「创新」和「LTS」两版本 ([MySQL Releases: Innovation and LTS](https://cunzaima.cn/mysql8.4-zh/dev.mysql.com/doc/refman/8.4/en/mysql-releases.html))，这跟 [Java](../../Java/Java_Note.md) 的版本策略类似。

「创新版」为短期「试验性」版本，而真正能用于生产环境的是「LTS」版本。

LTS 版本将遵循 [Oracle 终身支持政策](https://link.zhihu.com/?target=https%3A//www.oracle.com/support/lifetime-support/resources.html)，包括 5 年的首要支持和 3 年的延长支持，也就是 8 年的支持。

未来的计划：8.4.x、9.7.x 是 LTS，其余 8.x 及 9.x 是「创新」版。

> [!info] 
> 
> 在 MySQL 的官方规划中，约每隔两年会发布一个新的 LTS（长期稳定版）。
>
> 在一个长期稳定版发布后，则会按季度为单位持续发布创新版，每七个创新版后就会发布一个 LTS 版本。

![mysql version schedule screenshot](https://developer.qcloudimg.com/http-save/10653659/60a817fc9e60ec8daebd29fe56699ab8.png)

[MySQL各版本发布时间轴](https://docs.google.com/spreadsheets/u/0/d/e/2PACX-1vSs5aqTLLVWZGS6PnASYtZiyEupJTAnmNRV9tAVtNSX98xKmiNGt_eKfnd2rCT2C0LRVA6UHIVUA0AU/pubhtml?gid=1117405558&single=true&pli=1)

---

## 相关笔记

* [MySQL资料](MySQL_Material.md)
* [Linux 下安装 MySQL](Linux下安装MySQL.md)
* [Windows 下 MySQL 笔记](MySQL_Windows_Note.md)
* [MySQL常用操作](MySQL常用操作.md)
* [MySQL配置笔记](MySQL_Config_Note.md)
* [Docker 安装 MySQL](../../Docker/Docker_Note.md#dk_softc_demo_mysql)
* [MySQL 视频清单](MySQL_Videos.md)