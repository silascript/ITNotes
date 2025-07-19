---
aliases: []
tags:
  - database
  - db
  - mysql
  - windows
  - scoop
created: 2025-07-19 10:02:18
modified: 2025-07-19 10:16:58
---

# Windows 下 MySQL 笔记

---

## mysql 服务

### 添加服务

使用 [Scoop](../../Scoop/Scoop_Note.md) 安装 mysql ，安装完后，会有类似提示：`mysqld --install MySQL --defaults-file="F:\scoop\locals\apps\mysql-lts\current\my.ini"`

这就是要你安装 MySQL 的服务。`mysqld` 是 mysql 给的一个管理工具，已经封装有服务相关的操作。

`mysql --install 服务名`，这个服务名可以自定义。`--default-file` 这个参数是指定配置文件。

例：`mysqld --install MySQL84 --defaults-file="F:\scoop\locals\apps\mysql-lts\current\my.ini"`

> [!info]
> 
> 这个操作，PowerShell 或其他「命令行」工具得以「管理员身份运行」，因为这是非常底层的操作。不然会出现添加「不成功」：`Install/Remove of the Service Denied!` 这种提示。**添加成功**的提示是这样的：`Service successfully installed.`

`--install` 相关命令：

```shell
--install                     Install the default service (NT).
--install-manual              Install the default service started manually (NT).
--install service_name        Install an optional service (NT).
--install-manual service_name Install an optional service started manually (NT).

--enable-named-pipe           Only to be used for the default server (NT).
--standalone                  Dummy option to start as a standalone server (NT).
```

> [!info] 
> 
> `--install` 和 `--install-manual` 后面如果不带上自定义服务名，则使用默认服务名 -- 好像是「**MySQL**」。
>
> `--install-manual` 是让服务「手动」启动，如果使用 `--install` 命令，服务则是自动启动，如果想改手动，得到「服务管理器」那里手动改。
>
> 例：`mysqld --install-manual MySQL84 --defaults-file="F:\scoop\locals\apps\mysql-lts\current\my.ini"`
>

### 删除服务

删除服务相关命令：

```shell
--remove                      Remove the default service from the service list (NT).
--remove service_name         Remove the service_name from the service list (NT).
```

`--remove` 和 `--remove 服务名`，顾名思义就是删除 MySQL 相关服务。

> [!info] 
> 
> 当然也可以使用 Windows 系统本身的 `sc` 命令对 MySQL 服务进行 [删除](#删除服务) 操作。
>
> 语法：`sc delete 服务名`
>
> 例：`sc delete MySQL8`

---

## 配置

`my.ini` 配置示例：

```ini
[mysqld]
datadir=F:/scoop/locals/persist/mysql/data
port=3366

character-set-client-handshake = FALSE
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4'

[client]
user=root
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

```

---

## 相关笔记

* [Scoop 笔记](../../Scoop/Scoop_Note.md)
* [MySQL 笔记](MySQL_Note.md)

