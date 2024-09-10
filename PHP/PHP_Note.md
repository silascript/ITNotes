---
aliases: []
tags:
  - PL
  - php
created: 2023-09-21 23:13:13
modified: 2024-09-10 23:19:53
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

---

## Composer

### 下载安装

1. 下载 Composer（安装前请务必确保已经正确安装了 PHP。打开命令行窗口并执行 `php -v`​ 查看是否正确输出版本号。）
    
    下载安装脚本 － composer-setup.php​ － 到当前目录

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

4. 安装完成后会有 composer.phar 文件，运行 `php composer.phar​` 就可以查看 composer

### composer 分为局部安装和全局安装

#### 局部安装
    
可以将 `composer.phar` 文件复制到任意目录（比如项目根目录下），然后通过 `php composer.phar`​ 指令即可使用 Composer 了！

#### 全局安装

​`sudo mv composer.phar /usr/local/bin/composer​`

将当前目录中的 `composer.phar` 这个可执行文件移动到 `/usr/local/bin` 目录下，并更名为 `composer`，这是为了方便在系统任何地方都能调用 composer。

这样操作之后，就可以通过 `composer` 命令使用 composer 了 -- 当前因为放到了根目录下，所以使用 `sudo`。

> [!info] 
> 
> 如果在 [Docker](../Docker/Docker_Note.md) 里的 composer 可以不用 `sudo​`
    
可以通过 `whereis composer​` ​来查看当前 composer 的位置，看是不是 mv​ ​成功了。

### Composer 相关命令

composer 升级：

```shell
composer selfupdate
```

更新所有包：

```shell
composer update
```

更新指定包：

```shell
composer update 包名
```

安装包：

1. 创建一个 composer.json
    
    ```json
    {
    	"require":{
    		"monolog/monolog": "1.0.*"
    	}
    }
    ```
    
2. 运行 composer install​

清理缓存：

```shell
composer clear
```

诊断命令：

```shell
composer diagnose
```

查看全局配置：

```shell
composer config -gl
```

### Composer 镜像

#### 全局设置

配置镜像（以阿里为例）：

```shell
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```

> [!info] 
> 
> `​config -g​`：全局设置

取消镜像：

```shell
composer config -g --unset repos.packagist
```

#### 局部配置

配置镜像（以阿里为例）：

```shell
composer config repo.packagist composer https://mirrors.aliyun.com/composer/
```

取消配置：

```shell
composer config --unset repos.packagist
```

#### 常用镜像

* 阿里云 Composer 全量镜像
	* 镜像地址：[https://mirrors.aliyun.com/composer/](https://mirrors.aliyun.com/composer/)
	* 官方地址：[https://developer.aliyun.com/composer](https://developer.aliyun.com/composer)
* Packagist / Composer 中国全量镜像
	* 镜像地址：[https://packagist.phpcomposer.com](https://packagist.phpcomposer.com)
	* 官方地址：[https://pkg.phpcomposer.com](https://pkg.phpcomposer.com)
* Composer / Packagist 中国全量镜像
	* 镜像地址：[https://php.cnpkg.org](https://php.cnpkg.org)
	* 官方地址：[https://php.cnpkg.org](https://php.cnpkg.org)
* Packagist / JP
	* 镜像地址：[https://packagist.jp](https://packagist.jp)
	* 官方地址：[https://packagist.jp](https://packagist.jp)
* Packagist Mirror
	* 镜像地址：[https://packagist.mirrors.sjtug.sjtu.edu.cn](https://packagist.mirrors.sjtug.sjtu.edu.cn)
	* 官方地址：[https://mirrors.sjtug.sjtu.edu.cn/packagist](https://mirrors.sjtug.sjtu.edu.cn/packagist/)
* Laravel China Composer 全量镜像
	* 镜像地址：[https://packagist.laravel-china.org](https://packagist.laravel-china.org)
	* 官方地址：[https://learnku.com/laravel](https://learnku.com/laravel)
	> [!info] 
	> 
	> 说明：这个就不多了，国内 PHP 开发者使用量最多的 composer 镜像，同步速度快、稳定，推荐使用。

---

## 语法

---

## 其他相关笔记

* [Linux下PHP安装](Linux下PHP安装.md)
* [Linux下安装配置Apache](Linux下安装配置Apache.md)
* [PHP 视频清单](PHP_Videos.md)
* [PHP 资料清单](PHP_Material.md)
* [PHP 扩展笔记](PHP扩展.md)
* [Docker 笔记](../Docker/Docker_Note.md)
