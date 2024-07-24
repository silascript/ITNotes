---
aliases: []
tags:
  - mysql
  - db
  - database
  - config
created: 2024-07-24 18:49:11
modified: 2024-07-24 18:52:54
---
# MySQL 配置笔记

---

## 配置文件

### my.cnf 配置

my.cnf 示例：
```cnf
[mysqld]
basedir=/usr/local/mysql-5.7
datadir=/usr/local/mysql-5.7/data

port=3356

character-set-server=utf8
collation-server=utf8_general_ci

explicit_defaults_for_timestamp=true


log-error=/usr/local/mysql-5.7/data/mysqld.log
pid-file=/usr/local/mysql-5.7/data/mysql.pid


```

mysql5.6.6+ 版本，推荐加上 **explicit_defaults_for_timestamp=true**

utf8mb4 cnf 配置示例：

```cnf
[client]
default-character-set = utf8mb4

[mysql]
default-character-set = utf8mb4

[mysqld]
basedir=/opt/mysql_5.7/
datadir=/opt/mysql_5.7/data
port=3356

character-set-client-handshake = FALSE
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
init_connect='SET NAMES utf8mb4'

```

> **mysql_install_db** 不创建默认的 `my.cnf` 文件
>
> 从 MySQL 5.7.18 开始，`my-default.cnf` 不再包含在分发包中或由分发包安装。
> 
> 
> `character-set-client-handshake`，这是设置客户端链接时指定字符集，如果想要在链接时指定字符集，这个选项就不能设。
> 

### 各配置文件路径及优先级

|      File Name      |                    Purpose                    |
| :-----------------: | :-------------------------------------------: |
|     `/etc/my.cnf`     |                Global options                 |
|  `/etc/mysql/my.cnf`  |                Global options                 |
|  `SYSCONFDIR/my.cnf`  |                Global options                 |
| `$MYSQL_HOME/my.cnf`  |     Server-specific options(server only)      |
| defaults-extra-file |                                               |
|      `~/.my.cnf`      |             User-specific options             |
|   `~/.mylogin.cnf`    | User-specific login path options(client only) |

> [!info]
>
> MySQL 实例启动需要依赖 `my.cnf` 配置文件，而配置文件可以存在于多个操作系统目录下。
>
> `my.cnf` 的默认查找路径，从上往下找到的文件先读，但优先级逐级提升。

MySQL 8.0 开始，客户端的配置放在 `conf.d` 目录下的 `mysql.cnf` 文件。

---

## 字符集设置

因为 8.x 开始，MySQL 已经默认设置服务器端为 `utf8mb4`​，所以真正需要自己手动设置的只有客户端的字符集。所以在 8.x 及以上版本中，`[mysqld]​` ​里的字符集设置是不需要的，即 `character-set-server`​ ​和 `collation_server​` ​已经不用再配了。

客户端是没有设置的，需要用户自行设置。

```
[mysql]
default_character_set=utf8mb4

[client]
default_character_set=utf8mb4
```

> [!info] 
> 
> `[client]` 是客户端设置，而 `[mysql]` 其实也是客户端之一，是 MySQL 自带的客户端。
> 
> `default_character_set` 这行代码就是设置默认字符编码。
> 
> `utf8mb4` 对应 UTF-8，这才是「完整体」的 UTF-8。
> 
> 原来的「阉割版」MySQL 的 `utf8` 被重命名为 `utf8mb3`，原因是它的码长度最多只有三个字符。 

设置完后，重启 MySQL。进入 MySQL 后，使用 `status` 命令，可以查看相关信息，看配置是否成功。
其中 `Client characterset` 和 `Conn.  characterset` 就是客户端的字符编码。

```shell
mysql  Ver 8.0.38 for Linux on x86_64 (MySQL Community Server - GPL)

Connection id:		8
Current database:	
Current user:		silascript@localhost
SSL:			Not in use
Current pager:		stdout
Using outfile:		''
Using delimiter:	;
Server version:		8.0.38 MySQL Community Server - GPL
Protocol version:	10
Connection:		Localhost via UNIX socket
Server characterset:	utf8mb4
Db     characterset:	utf8mb4
Client characterset:	utf8mb4
Conn.  characterset:	utf8mb4
UNIX socket:		/var/run/mysqld/mysqld.sock
Binary data as:		Hexadecimal
Uptime:			54 sec
```

##### 修改现有字符集

对现有的库、表、列的字符集，是可以通过 alter​命令对修改。

###### 修改数据库的字符集

```sql
alter database 数据库名 character set utf8mb4;
```

###### 修改表的字符集

```sql
alter table 表名 character set utf8mb4;
```

###### 修改列的字符集

```sql
alter table 表名 change 列名 列名 varchar(10) character set utf8mb4;
```

---

## 相关笔记

* [MySQL 笔记](MySQL_Note.md)
* [linux下安装mysql](linux下安装mysql.md)

