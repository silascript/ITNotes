---
aliases: []
tags:
  - php
  - install
  - windows
created: 2025-11-14 04:26:58
modified: 2025-11-14 20:14:15
---

# Windows 下安装 PHP

---

## 配置 PHP

[PHP](../PHP_Note.md) 配置文件是 `php.ini`。

### 配置扩展

#### 指定扩展功能目录

```ini
extension_dir = "ext"
```

#### 开启相应的动态扩展

动态扩展：「Dynamic Extensions」

1. 开启 [MySQL](../../DataBase/mysql/MySQL_Note.md) 支持：

```ini
	extension=php_mysql.dll
	extension=php_mysqli.dll
	# 以pdo方式mysql
	extension=php_pdo_mysql.dll
```

2. 多字节文本支持：

```ini
# extension=php_mbstring.dll
extension=mbstring
```

3. l 图片相关支持：

```ini
# gd2用于处理图像、例如创建缩略图、裁剪图片、制作验证码图片等
extension=gd2

# 打启exif 可以处理图片的元数据。例如：可以读取数码相机照到的照片的元数据。  
# EXIF（Exchangeable image file format）是可交换图像文件的缩写
# 是专门为数码相机的照片设定的，可以记录数码照片的属性信息和拍摄数据。  
# EXIF可以附加于JPEG、TIFF、RIFF等文件之中
# 为其增加有关数码相机拍摄信息的内容和索引图或图像处理软件的版本信息。
# 此扩展依赖上面的mbstring，所以必须放在mbstring下面
extension=php_exif.dll
```

### 打开显示错误信息功能

```ini
display_errors = On
```

> [!info] 
> 
> 建议开发模式下，打开此功能

### 开启 PATH_INFO 信息

在 cgi 模式下，可以开启 `PATH_INFO` 功能：

```ini
cgi.fix_pathinfo=1
```

---

## 运行 PHP

### 运行 php-cgi

在运行 `php-cgi.exe` 前，建议 [开启 PATH_INFO 信息](#开启%20PATH_INFO%20信息) 功能。

```shell
php-cgi.exe -b 127.0.0.1:9000
```
> [!info] 
> 
> `-b` 指定 ip 地址及端口，默认端口号为 `9000`，这个端口号是给 [nginx](#nginx) 用的，得在 [配置nginx](#配置nginx) 的 `fastcgi_pass` 属性时指定。

#### 指定 php.ini 配置文件

如果要指定使用配置文件，可以添加 `-c` 参数：

```shell
php-cgi.exe -b 127.0.0.1:9000 -c xx/xx/php.ini
```

### 查看 php-cgi 进程

```shell
tasklist /fi "imagename eq php-cgi.exe"
```

---

## Nginx

### 配置 Nginx

与在 [Linux下配置Nginx](../../Docker/Docker_Examples.md#nginx%20配置文件) 基本一样。

`nginx.conf` 文件示例：

```conf

server {
        listen       8899;         //端口号（默认80，因已存在一个集成环境造成冲突，改成81），根据自己需要修改
        server_name  test.com;   //喜欢什么写什么（记得在host文件上加上该域名）

        #charset utf-8;

        #access_log  logs/host.access.log  main;

        location / {
            root   E:\self\www;    //修改成自己网站根目录的绝对路径（自己喜欢）
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        # 把这几个前面的注释#符号删掉
　　　　　location ~ \.php$ { 

　　　　　　　#网站根目录（跟上面那个一样）
　　　　　　　root E:/self/www; 

　　　　　　	# php-cgi 监听端口号（默认9000，根据实际情况自己修改）     
　　　　　　　fastcgi_pass 127.0.0.1:9000;   
　　　　　　　fastcgi_index index.php;

　　　　　　　#下面这里要改看清楚原本是/script$fastcgi_script_name，改成$document_root$fastcgi_script_name;  

　　　　　　　# $document_root其实就是上面的 root
			# 可以直接改成绝对路径E:/self/www$fastcgi_script_name 这样子
　　　　　　　fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;  
　　　　　　　include fastcgi_params;

　　　　}
}

```

> [!info] 
> 
> `root` 这个是网站根目录，会有 `fastcgi_param` 属性中使用 `$document_root` 来引用。
>> [!tip] 
>> 
>> 当然也可以不使用 `root` 属性，直接在 `fastcgi_param` 设置时直接将路径写入，如这样：`fastcgi_param  SCRIPT_FILENAME  /var/www/html/$fastcgi_script_name;`。
> 
> `fastcgi_pass` 这个属性是最重要的，告诉 [Nginx](../../Network/Nginx/Nginx_Note.md) 如何调 [PHP](../PHP_Note.md) 引擎来解析 php 文件。

### 运行 Ngnix

```shell
# 开启 nginx
start nginx
```

> [!info] 
> 
> 在终端中执行 start nginx 后，会弹出一个 nginx 空的窗口，就算关闭此窗口，nginx 的进程仍在运行，可以使用任务管理器查看到。
>
>如果需要真正关闭 nginx，需要 nginx -s stop 命令

#### 其他 Nginx 命令

* 停止：`nginx -s stop` 
* 重启：`nginx -s reload` 
* 结束：`nginx -s quit`

> [!tip]
>  
> 如果关闭不了，而任务管理器中 `nginx.exe` 进程还存在，可以使用进程相关操作进行关闭

* 查看 nginx 进程
```shell
tasklist /fi "imagename eq nginx.exe"
```

* 彻底停止 nginx 进程
```shell
taskkill /f /t /im nginx.exe
```

---

## Apach

### 配置 Apache

`httpd.conf` 是 [Apache](https://httpd.apache.org) 的主配置文件。

常用配置有如下这几项：

#### 设置端口
	
示例：`Listen 9988`

#### 指定服务器根目录

示例：`ServerRoot "I:/soft/phpsuit/Apache24"`

#### 加载 [PHP](../PHP_Note.md) 模块及指定 PHP 安装目录

示例：

```properties
LoadModule php5_module "I:/soft/phpsuit/php55/php5apache2_4.dll"
PHPIniDir "I:/soft/phpsuit/php55"
```

#### 指定 `DocumentRoot` 并配置 `Directory` 节点
示例：

```properties
DocumentRoot "I:/soft/phpsuit/Apache24/htdocs"
<Directory "J:/PHPExercise/">
```

#### 添加 Apache 服务器识别 php 类型
	
示例：`AddType application/x-httpd-php .php`

#### 加载虚拟目录的配置文件

示例：`Include conf/extra/httpd-vhosts.conf`

### 辅配置文件

辅配置文件是 Apache 主配置文件 `httpd.conf` 的扩展文件，用于将一部分配置抽取出来，以便于修改，但默认并没有启用，所以要想使用辅配置文件，得先启用。

在 `httpd.conf` 主配置文件中使用 `Include` 方式，启用导入或称其为「加载」辅配置文件，如 [加载虚拟目录的配置文件](#加载虚拟目录的配置文件)。

#### 配置虚拟主机

> [!info] 
> 
> 要想虚拟主机的配置生效，得先启用这个 [辅配置文件](#辅配置文件)，即在主配置文件 `httpd.conf` 中 [加载虚拟目录的配置文件](#加载虚拟目录的配置文件)。

配置虚拟主机，是在 `httpd-vhost.conf` 配置文件中配置。

示例：

```xml
<VirtualHost *:9988>
    DocumentRoot J:/PHPExercise
</VirtualHost>
```

> [!tip] 
> 
> 指定虚拟目录路径及端口时，其路径结尾不带 `/`

### 运行 Apache

---

## 相关笔记

* [PHP_Note](PHP_Note.md)
* [PHP_Material](PHP_Material.md)

