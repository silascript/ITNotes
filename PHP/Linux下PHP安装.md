---
aliases: 
tags: linux php
created: 2023-01-13, 12:27:46
modified: 2023-01-30, 9:18:05
---
# Linux 下 PHP 安装

---

## 目录
* [php 安装](#linux_php_install)
* [Docker 下安装 PHP](#linux_php_docker)


---

## <span id="linux_php_install">PHP 安装</span>


### 从源码安装

#### 1. 从官网下载压缩包 解压，进入解压的源码目录中

如下图：

![2020-05-30 00-42-17 屏幕截图](./Linux 下 PHP 安装.assets/2020-05-30 00-42-17 屏幕截图.png)



#### 2. 配置

```shell
    ./configure --prefix=/usr/local/PHP/php7.3 --exec-prefix=/usr/local/PHP/php7.3 --bindir=/usr/local/PHP/php7.3/bin --with-config-file-path=/usr/local/PHP/php7.3/etc --with-apxs2=/usr/local/apache2.4/bin/apxs --enable-mysqlnd --with-mysqli --with-pdo-mysql --enable-exif --enable-mbstring --with-openssl --enable-session 
	
```

###### PHP 各版本对于 mysql 驱动的不同设置

PHP 从 5.4 开始内置了 MySQL 驱动 mysqlnd:

php-src/ext/mysqlnd/

php-src/ext/mysql/

php-src/ext/mysqli/

php-src/ext/pdo_mysql/

关系:mysql,mysqli,pdo_mysql 这 3 套 PHP 操作 MySQL 的编程接口底层都依赖 PHP 内置的 MySQL 驱动 mysqlnd.

PHP5.3 这样启用 mysqlnd 支持:
```shell
--with-mysql=mysqlnd
--with-mysqli=mysqlnd
--with-pdo-mysql=mysqlnd
```

**PHP5.4 后留空则默认启用 mysqlnd**:
```shell
--with-mysql
--with-mysqli
--with-pdo-mysql
```

**PHP7 开始不再支持 --with-mysql**


#### 3. make

#### 4. make install



### 使用 phpize 来编译和安装 php 的扩展



#### phpize 在 php 安装目录中的 bin 目录下

到 php 的源码目录下 extensions（扩展）目录，phpize 必须在想要编译安装的扩展的目录中

###### 大致步骤：

1. 在相应的扩展源码目录中敲入 phpize 命令

   如果没有错误 phpize 在此扩展源码中生成相应的 configure 文件

   使用 configure 命令进行编译前配置

```shell
./configure --with-php-config=/usr/local/php/bin/php-config  #配置时 要将php-config的路径附上
```

`--with-php-config` 用来指定 php-config 的路径，php-config 是 php 安装目录中 bin 目录中的一个跟 php 配置相关的程序



**如果是 mysql 扩展，得再加其他配置选项**

php-5.3.x: 

```shell
./configure --with-mysql=mysqlnd \ --with-mysqli=mysqlnd \ --with-pdo-mysql=mysqlnd \ 
```


php-5.4+

php 5.4 及以上编译时可以不加 mysqlnd 默认即为 mysqlnd

PHP7 正式移除了 `mysql` 扩展

[编译安装PHP的参数 --with-mysql --with-mysqli --with-apxs2默认路径](https://www.cnblogs.com/meiling12/p/6096789.html)

编译安装 PHP 时指定如下几个参数说明：
```shell
--with-apxs2=/usr/local/apache/bin/apxs # 整合apache，apxs功能是使用mod_so中的LoadModule指令，加载指定模块到apache，要求apache要打开SO模块
--with-config-file-path=/usr/local/php/etc # 指定php.ini位置
--with-MySQL=/usr/local/mysql # mysql安装目录，对mysql的支持
--with-mysqli=/usr/local/mysql/bin/mysql_config # mysqli扩展技术不仅可以调用MySQL的存储过程、处理MySQL事务，而且还可以使访问数据库工作变得更加稳定。
```
 
> 以上参数指定的都是我们编译安装 apache,mysql 的路径位置，如果在环境只编译安装 PHP，apache 和 mysql 用系统自带 yum 安装 rpm 相关的包；

这时在编译安装 PHP 的时候指定的默认路径如下：

```shell
--with-apxs2=/usr/sbin/apxs
--with-config-file-path=/etc
--with-mysql=/usr
--with-mysqli=/usr/bin/mysql_config
```


> 若用 which apxs 查找不到路径的话，可能是没有安装 http-devel 这个包，安装一下就好。

 

3. make
4. make install



---

## <span id="linux_php_docker">Docker 下安装 PHP</span>

如果想快速安装使用 php，可以考虑使用 Docker。

Docker 下如何安装 PHP，请参考：[Docker 安装 PHP](../Docker/Docker_Note.md#dk_softc_demo_php)






