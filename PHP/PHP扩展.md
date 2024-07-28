---
aliases: []
tags:
  - PL
  - php
  - extension
created: 2024-07-28 12:14:53
modified: 2024-07-28 12:19:09
---

# PHP 扩展

---

## PECL

PHP 扩展社区库（英语：The PHP Extension Community Library，简称 PECL，读作 pickle），是一个 PHP 扩展库，提供了一个 PHP 所有已知扩展的下载和托管目录。PECL 通过 PEAR 进行打包和安装。

PECL 是 <span data-type="text" id="">PHP 笔记</span>的标准扩展，可以补充实际开发中所需的功能，所有的扩展都需要安装，在 Windows 下面以.dll 文件的形式出现，而在 Linux 下面，需要单独进行编译，它的表现形式为：根据 PHP 官方的标准，用 C 或 C++ 语言写成，尽管源码开放，但是一般人无法随意更改源码。

‍

---

## Composer

### 下载安装

1. 下载 Composer（安装前请务必确保已经正确安装了 PHP。打开命令行窗口并执行 `php -v`​ 查看是否正确输出版本号。）

   下载安装脚本 － `composer-setup.php`​ － 到当前目录

   ```shell
   php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');"
   ```

2. 执行安装过程

   ```shell
   php composer-setup.php
   ```

3. 删除安装脚本

   ```shell
   php -r "unlink('composer-setup.php');"
   ```

4. 安装完成后会有 composer.phar 文件，运行 `php composer.phar`​ 就可以查看 composer

‍

### composer 分为局部安装和全局安装

1. 局部安装

   可以将 composer.phar 文件复制到任意目录（比如项目根目录下），然后通过 `php composer.phar`​ 指令即可使用 Composer 了！

2. 全局安装

   ​`sudo mv composer.phar /usr/local/bin/composer`​

   然后通过 composer 就可以使用 composer 了，不管是不是 root 用户，都要加上 sudo

   > [!info] 
   > 
   > 如果在 <span data-type="text" id="">Docker </span>里的 composer 可以不用 `sudo`​
   >

   可以通过 `whereis composer` ​来查看当前 composer 的位置，看是不是 `mv` ​成功了。

3. composer 版本升级

   ​`composer selfupdate`​

‍

### Composer 相关命令

‍

composer 升级：

```shell
composer selfupdate
```

‍

查看全局配置：

```shell
composer config -gl
```

‍

‍

‍

---

## 相关笔记

* [PHP笔记](PHP_Note.md)