---
aliases: []
tags:
  - PL
  - php
  - extension
created: 2024-07-28 12:14:53
modified: 2024-07-28 18:26:48
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

	下载安装脚本 － `composer-setup.php`​ － 到当前目录：
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

	`sudo mv composer.phar /usr/local/bin/composer`​
	
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

#### composer 升级

```shell
composer selfupdate
```

#### 包操作

#### 更新所有包

```shell
composer update
```

##### 更新指定包

```shell
composer update 包名
```

#### 安装包

1. 创建一个 composer.json
    
    ```json
    {
    	"require":{
    		"monolog/monolog": "1.0.*"
    	}
    }
    ```
    
2. 运行 `composer install`​

#### 清理缓存

```shell
composer clear
```

#### 诊断命令

```shell
composer diagnose
```

#### 查看全局配置

```shell
composer config -gl
```

‍
### Composer 镜像

#### 全局设置

配置镜像（以阿里为例）：

```shell
composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/
```
> [!tip] 
> 
> ​config -g​：全局设置

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
‍

‍

---

## 相关资料

#### 镜像相关

* [中国全量镜像](https://pkg.xyz)
* [阿里云 Composer 全量镜像](https://developer.aliyun.com/composer)
* [Composer 国内加速，修改镜像源](https://learnku.com/articles/15977/composer-accelerate-and-modify-mirror-source-in-china)

---

## 相关笔记

* [PHP笔记](PHP_Note.md)