---
aliases: []
tags:
  - PL
  - php
created: 2023-09-21 23:13:13
modified: 2024-08-16 11:28:11
---

# PHP 笔记

---

## 目录

* [版本](#版本)
* [内置服务器](#内置服务器)

---

## 版本

### 相关资料

* [PHP Versions • PHP.Watch](https://php.watch/versions)

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

内置服务器，可以在不配置 [Nginx](../Network/Nginx/Nginx_Videos.md) 或 [Apache](Linux下安装配置Apache.md) 服务器下，快速进行 php 开发或学习。但实际开发布署，还是得配下 nginx 或 apache。

---

## 扩展

## PECL

PHP 扩展社区库（英语：The PHP Extension Community Library，简称 PECL，读作 pickle），是一个 PHP 扩展库，提供了一个 PHP 所有已知扩展的下载和托管目录。PECL 通过 PEAR 进行打包和安装。

PECL 是 PHP 笔记的标准扩展，可以补充实际开发中所需的功能，所有的扩展都需要安装，在 Windows 下面以.dll 文件的形式出现，而在 Linux 下面，需要单独进行编译，它的表现形式为：根据 PHP 官方的标准，用 C 或 C++ 语言写成，尽管源码开放，但是一般人无法随意更改源码。

### 扩展相关资料

* [安装PHP的GD扩展 - 易文杰 - 博客园](https://www.cnblogs.com/ywjcqq/p/14717328.html)
* [GD 安装与配置 - Manual](https://www.php.net/manual/zh/image.installation.php)

---

## 相关资料

* [Phpstorm Xdebug 远程调试代码 – Zgao's blog](https://zgao.top/phpstorm-xdebug-%E8%BF%9C%E7%A8%8B%E8%B0%83%E8%AF%95%E4%BB%A3%E7%A0%81/)

---

## 其他相关笔记

* [Linux下PHP安装](Linux下PHP安装.md)
* [Linux下安装配置Apache](Linux下安装配置Apache.md)
* [PHP 视频清单](PHP_Videos.md)
* [PHP 扩展笔记](PHP扩展.md)
* [Docker 笔记](../Docker/Docker_Note.md)
