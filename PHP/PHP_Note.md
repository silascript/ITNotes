---
aliases:
  - 
tags:
  - 
created: 2023-09-21 23:13:13
modified: 2023-09-21 23:20:58
---
# PHP 笔记

---

## 内置服务器

启动 php 的内置服务器：

```shell
php -S localhost:端口号
```

如果要指定 php 的网站根目录：

```shell
php81 -S localhost:8088 -t /home/silascript/DevWorkSpace/PHPExercise
```

> [!info]
> 
> `-t` 参数就是指定网站根目录的路径

内置服务器，可以在不配置 [Nginx](../DataBase/mysql/Nginx_Videos.md) 或 [Apache](Linux下安装配置Apache.md) 服务器下，快速进行 php 开发或学习。但实际开发布署，还是得配下 nginx 或 apache。

---

## 其他相关笔记

* [Linux下PHP安装](Linux下PHP安装.md)
* [Linux下安装配置Apache](Linux下安装配置Apache.md)
