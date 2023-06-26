---
aliases:
  - 
tags:
  - database
  - mysql
created: 2023-01-30 11:19:11
modified: 2023-06-22 7:58:12
---
# MySQL 笔记

---

## 目录

* [客户端工具](#mysqln_client_tools)
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
> 官方提供了几个 [配色](https://www.mycli.net/syntax)。

### 连接问题
如果在连接 MySQL 8.x 时，出现 `Public Key Retrieval is not allowed` 错误，请开启相应的权限。 

---

## 相关内容

* [Linux 下安装 MySQL](./linux下安装mysql.md)
* [MySQL常用操作](./MySQL常用操作.md)
* [Docker 安装 MySQL](../Docker/Docker_Note.md#dk_softc_demo_mysql)
* [MySQL 视频清单](./MySQL_Videos.md)