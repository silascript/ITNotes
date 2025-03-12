---
aliases: []
tags:
  - mysql
  - db
  - database
  - config
created: 2024-07-24 18:49:11
modified: 2025-03-13 02:01:22
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

mysql5.6.6+ 版本，推荐加上 `explicit_defaults_for_timestamp=true`

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

>[!tip] 
>
> **mysql_install_db** 不创建默认的 `my.cnf` 文件
>
> 从 MySQL 5.7.18 开始，`my-default.cnf` 不再包含在分发包中或由分发包安装。
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

### MySQL 数据库中字符集转换流程

* [MySQL](MySQL_Note.md) 收到请求时将请求数据从 `character_set_client`​ 转换为 `character_set_connection`​
    
* 进行内部操作前将请求数据从 `character_set_connection`​ 转换为内部操作字符集，其确定方法如下：
    * 使用每个数据字段的 `CHARACTER SET​` 设定值
    * 若上述值不存在，则使用对应数据表的 `DEFAULT CHARACTER SET`​ 设定值 (MySQL 扩展，非 SQL 标准)
    * 若上述值不存在，则使用对应数据库的 `DEFAULT CHARACTER SET`​ 设定值
    * 若上述值不存在，则使用 `character_set_server`​ 设定值
* 将操作结果从内部操作字符集转换为 `character_set_connection​`
    
* 将响应数据从 `character_set_connection`​ 转为 `character_set_client`​

> [!info] 
> 
> 执行 SQL 语句时信息的路径是这样的：
> 
> 信息输入路径：client → connection → server
> 
> 信息输出路径：server → connection → results
> 
> 所以只需要关注 client、connection、server 和 results 这四个位置的字符集设置，那从输入到输出的字符集设置就是确定的。
> 
> 要知道这四处位置字符集信息，可以通过 status​ ​命令查询到。

### 查询字符集信息

* ​status​ ​命令查询：

```shell
mysql> status;
--------------
mysql  Ver 8.0.38 for Linux on x86_64 (MySQL Community Server - GPL)

Connection id:		9
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
Uptime:			12 sec
```

以下四个属性值均为 `utf8mb4`​ ​就 OK 了：

* ​`Server characterset​`
* ​`Db characterset​`
* ​`Client characterset​`
* ​`Conn.characterset​`

* 使用 `show variables like "%char%";​`：查询字符集相关配置信息：

```shell
+--------------------------+--------------------------------+
| Variable_name            | Value                          |
+--------------------------+--------------------------------+
| character_set_client     | utf8mb4                        |
| character_set_connection | utf8mb4                        |
| character_set_database   | utf8mb3                        |
| character_set_filesystem | binary                         |
| character_set_results    | utf8mb4                        |
| character_set_server     | utf8mb3                        |
| character_set_system     | utf8mb3                        |
| character_sets_dir       | /usr/share/mysql-8.0/charsets/ |
+--------------------------+--------------------------------+
```

* ​`character_set_client`​：客户端请求数据的字符集
* ​`character_set_connection`​：从客户端接收到数据，然后传输的字符集
* ​`character_set_database​`：默认数据库的字符集，无论默认数据库如何改变，都是这个字符集；如果没有默认数据库，那就使用 `character_set_server`​ ​指定的字符集，这个变量建议由系统自己管理，不要人为定义。
* `​character_set_filesystem​`：把操作系统上的文件名转化成此字符集，即把 `character_set_client`​ 转换 `character_set_filesystem`​， 默认 binary 是不做任何转换的
* ​`character_set_results​`：结果集的字符集
* ​`character_set_server`​：数据库服务器的默认字符集
* ​`character_set_system​`：存储系统元数据的字符集，总是 utf8，不需要设置。

### 服务器端字符集设置

```conf
[mysqld]
character-set-server=utf8mb4
```

`​character-set-server​` 这个配置项是配置服务端的字符集。受其影响的有：

* ​`character_set_database​`
* ​`character_set_server​`

对于客户端设置，应设 `default_character_set​` ​配置项。

```cnf
[mysql]
default_character_set=utf8mb4

[client]
default_character_set=utf8mb4
```

​`default_character_set​` ​设置，受影响的有：

* ​`character_set_client​`
* ​`character_set_connection​`
* `​character_set_results`​

### 关于 UTF8

MySQL 如果在字符集设置时，将值设为 utf8​，那最终在 MySQL 8.x 后，数据库系统同查询到的设置值为 `utf8mb3​`。

原因是 MySQL 在 8.x 版本之前的 utf8，是「残血版」，只支持到三个字节的 utf8。在 8.x 后，这个「残血版」的 utf8 被修订为 `utf8mb3​`，即这里的 3​ ​指的就是「三字节」。真正「满血版」的 utf8，应该为 `utf8mb4`​，即四字节的 utf8。所以在对于 8.x 的 MySQL 配置，应该是配的是 `utf8mb4`​。

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

## 其他配置

### Time-zone

[MySQL8](MySQL_Note.md) 中存在时区设置问题。

可以使用 `show variables like '%time_zone%'` 语句查询当前数据库是否已经设置时区：

```shell
mysql> show variables like '%time_zone%';
+------------------+--------+
| Variable_name    | Value  |
+------------------+--------+
| system_time_zone | UTC    |
| time_zone        | SYSTEM |
+------------------+--------+
2 rows in set (0.01 sec)

```

默认 `time_zone` 被设置成 `UTC`，而使用 `serverTimezone=UTC` 会导致数据的时间保存到数据库时比传入的时间少 8 小时。

所以我们得重设时区：`serverTimezone=Asia/Shanghai`

#### 常用时区列表

|       时区       |     时区标识      |
|:----------------:|:-----------------:|
|   中国标准时间   |   Asia/Shanghai   |
| 美国东部标准时间 | America/New_York  |
| 澳大利亚悉尼时间 | Australia/Sydney  |
|   印度标准时间   |   Asia/Kolkata    |
| 英国格林尼治时间 |   Europe/London   |
| 加拿大多伦多时间 |  America/Toronto  |
|  巴西圣保罗时间  | America/Sao_Paulo |
|   韩国标准时间   |    Asia/Seoul     |
| 俄罗斯莫斯科时间 |   Europe/Moscow   |

#### 方式 1

如果是使用 [Java](../../Java/Java_Note.md) 的 [JDBC](../../Java/Java_Web_Note.md#JDBC) 访问 MySQL 时，可以通过在「访问 URL」中添加参数解决时区问题，如：`jdbc:mysql://localhost:3356/xxx?serverTimezone=Asia/Shanghai`。

#### 方式 2

直接在 mysql 的配置文件设置。在 `mysqld` 节点中添加以下配置：

```ini
[mysqld]
default-time-zone='Asia/Shanghai'
```

---

## 相关笔记

* [MySQL 笔记](MySQL_Note.md)
* [MySQL资料](MySQL_Material.md)
* [Linux下安装MySQL](Linux下安装MySQL.md)

