---
aliases: []
tags:
  - linux
  - archlinux
  - centos
  - fedora
  - ubuntu
  - tar
  - shell
  - network
created: 2023-08-18 19:44:52
modified: 2025-03-19 07:20:29
---

# Linux 笔记

---
## 目录

* [配置文件](#linux_configs)
* [目录](#linux_directory)
* [用户](#linux_user)

* [打包压缩](#linux_tarc)
  * [Tar 命令](#linux_tarc_tar)
  * [压缩工具](#linux_tarc_comptools)
    * [gzip](#linux_tarc_comptools_gzip)
    * [bzip2](#linux_tarc_comptools_bzip)
    * [compress](#linux_tarc_comptools_compress)
    * [XZ](#linux_tarc_comptools_xz)
* [文本处理](#linux_textprocessing)
	* [文本处理常用命令](#linux_textprocessing_commands)
	* [正则表达式](#linux_textprocessing_regex)
	* [grep](#linux_textprocessing_regex)
	* [sed](#linux_textprocessing_sed)
	* [awk](#linux_textprocessing_awk)
* [字体](#linux_font)
* [网络](#linux_network) ^dee618
  * [Linux 网络相关命令](#linux_network_command)
    * [ip](#linux_network_command_ip)
    * [curl](#linux_network_command_curl)
* [SSH 相关](#linux_ssh)
    * [安装与设置](#linux_ssh_insconf)
    * [SSH 各种问题](#linux_ssh_bugs)
* [Shell相关](#linux_shell)

* [部分软件安装设置](#linux_soft_installsetup)
  * [使用 desktop](#linux_soft_install_desktop)

* [图形界面相关](#图形界面相关)
	* [Xorg](#Xorg)
	* [Wayland](#Wayland)
* [其他Linux笔记](#linux_notes)
* [相关资源链接](#linux_resource_links)
---

## <span id="linux_terminal"> 终端 </span>

---

## <span id="linux_versions">版本</span>

### 内核版本

可以到内核官网查看内核信息：[Linux Kernel](https://kernel.org)

### <span id="linux_versions_query">查询系统版本</span>

#### cat 方式

```shell
cat /proc/version
```

#### uname

```shell
# 查询版本所有信息
uname -a

# 查询linux内核版本信息
uname -r

```

##### uname 选项

```shell
-a, --all                按如下次序输出所有信息，其中若 -p 和 -i 的
                             探测结果为未知，则省略：
-s, --kernel-name        输出内核名称
-n, --nodename           输出网络节点的主机名
-r, --kernel-release     输出内核发行号
-v, --kernel-version     输出内核版本号
-m, --machine            输出主机的硬件架构名称
-p, --processor          输出处理器类型（不可移植）
-i, --hardware-platform  输出硬件平台（不可移植）
-o, --operating-system   输出操作系统名称
    --help        显示此帮助信息并退出
    --version     显示版本信息并退出

```

> [!tip] 
> 
> `uname` 可以看到当前系统用到的 [内核版本](#内核版本)。

#### lsb_release

LSB 是 **Linux Standard Base**（Linux 标准库）的缩写， `lsb_release` 命令 用来与具体 Linux 发行版相关的 Linux 标准库信息。

> [!info] LSB 译法
> 
> LSB 的译法有 Linux 标准库，Linux 标准规范

显示所有版本信息：

```shell
lsb_release -a
```

lsb 其他选项和参数：

* `-r`：发行版版本号
* `-c`：发行版代号
* `-d`：发行版描述信息
* `-i`：发行商名称 ID
* `-v`：lsb 版本信息

---

## <span id="linux_configs"> 配置文件 </span>

* profile 文件

  profile（`/etc/profile`） 文件用于设置系统级的环境变量和启动程序，在这个文件下配置会对所有用户生效。
  
  `~/.profile` 这是用户级别的环境变量配置文件。

  大多数发行版使用的是 `~/.profile` 。因为 `~/.profile` 文件可以被所有 Shell 读取，而 `~/.bash_profile` 仅能被 **Bash Shell** 读取。

* bashrc

  bashrc 只会被 bash shell 调用。

  bashrc 文件分为两种级别：
    * 系统级：`/etc/bashrc`
    * 用户级：`~/.bashrc`

  bashrc 是交互式非登录 Shell 时被执行。

* bash_profile 文件

  bash_profile 文件是作为交互式登录 Shell 被调用情况下，被读取执行。

在登录 Shell 的 bash 环境时，会依序执行以下三个文件其中一个：
* `~/.bash_profile`
* `~/.bash_login`
* `~/.profile`

 根据使用者账号，于其 `homt` 目录内读取 `~/.bash_profile` ；

 读取失败则会读取 `~/.bash_login` ；

 再次读取失败则读取 `~/.profile` 
 > [!tip]
 > 这三个文件设定基本无差别，仅读取上有优先关系
 
> [!tip] path 变量路径
> 路径末尾不能以 **\/** 结尾，否则将导致整个 PATH 变量出错。

---

## <span id="linux_environment">环境变量</span>

### env

`env` 命令是用于显示系统中已存在的环境变量，以及在定义的环境中执行指令。

> [!info] 
> 
> [Shell](Shell/Shell_Note.md) 文件开头的那个 `#!/usr/bin/env bash` 命令，意思就是使用 `env` 去找 [Bash](Shell/Shell_Note.md#Bash) 解析器来解析当前的 Shell 脚本。

---

## <span id="linux_directory">目录</span>

----

## <span id="linux_user">用户</span>

更改用户组：

```shell
# -R 选项是表示递归文件或目录
chgrp -R 用户名 目录名
```

> [!tip] 
> 
> `-R`：表示递归，即当前目录名的所有文件及子目录其及文件均更改。下面那些 [更改用户名](#^linux-chown)，这个参数也同样适用。

更改用户名： ^linux-chown

```shell
# -R 选项是表示递归文件或目录
chown -R 用户名 文件名
# 连同用户组一起改
chown -R 用户名:用户组名 文件名
```

### 权限

`chmod` 命令是用来修改用户权限的。

---

## <span id="linux_tarc"> 打包压缩 </span>

Linux 下打包和压缩是分两部分的。

打包用的是 [Tar 命令](#Tar%20命令)。

[压缩工具](#压缩工具) 在 Linux 上常用的主要有以下四种：
* [gzip](#gzip)
* [bzip2](#bzip2)
* [compress](#compress)
* [XZ Utils](#XZ%20Utils)

### <span id="linux_tarc_tar">Tar 命令 </span>

Linux 下最常用的打包程序就是 tar。

tar 本身只能用来打包，不具备压缩能力。但 tar 可以与 gzip、bzip2 等压缩工具结合一起来用。

tar 包以 **.tar** 为后缀名。

tar 常用选项和参数：

* -f（--file）：指定存档或设备中的文件（默认是 「-」，表示 标准输入/输出）
* -v （--verbose）：显示文件处理过程
* -t （--list）：查看存档中的文件列表
* -C （--directory）：提取存档到指定目录
* -c （--create）：创建一个新的存档，即「压缩」
* -x （--extract）：从存档提取文件，即「解压」
* -r （--append）：将文件附加到存档结尾，即增加文件到包中
* -u （--update）：仅将较新的文件附加到存档中，即更新
> -u 只会添加 mtime 初修改过的文件；-r 直接添加，不检查 mtime。

---

### <span id="linux_tarc_comptools"> 压缩工具 </span>

#### <span id="linux_tarc_comptools_gzip">gzip</span>

gzip 是 GNU 组织开发的一个压缩程序，后缀名为 **.gz** 。

与 gzip 相对的解压的程序是 gunzip。

与 tar 配合使用，通过 **-z** 参数来调用。

使用示例：

```shell
# 打包并使用 gzip 压缩
tar -czvf xxx.tar.gz *.jpg

# 解压
tar -xzvf xxx.tar.gz

# 查看包内文件
tar -tzf xxx.tar.gz

# 如果加上v 可以看到包内文件的如权限等详细信息
tar -czvf xxx.tar.gz
tar -xzvf xxx.tar.gz

```

#### <span id="linux_tarc_comptools_bzip">bzip2</span>

**bzip2** 是一个压缩能力更强的压缩程序，后缀名为 **.bz2**。

与 bzip2 相对的解压程序是 **bunzip2**。

与 tar 配合使用，通过 **-j** 参数来调用。

使用示例：

```shell
tar -cjf xxx.tar.bz2 *.jpg

# 解压
tar -xjf xxx.tar.bz2

# 如果加上v 可以看到包内文件的如权限等详细信息
tar -cjvf xxx.tar.bz2
tar -xjvf xxx.tar.bz2
```

#### <span id="linux_tarc_comptools_compress">compress</span>

**compress** 也是一个压缩程序，不过用得比较少，后缀名为 **.Z**。

与 compress 相对的解压程序是 **uncompress**。

与 tar 配合使用，通过 **-Z** 参数来调用。

使用示例：

```shell
tar -cZf xxx.tar.Z *.jpg

# 解压
tar -xZf xxx.tar.Z

# 如果加上v 可以看到包内文件的如权限等详细信息
tar -cZvf xxx.tar.Z
tar -xZvf xxx.tar.Z
```

#### <span id="linux_tarc_comptools_xz">XZ Utils</span>

[XZ Utils](http://tukaani.org/xz/) 是使用 「LZMA」算法的无损数据压缩的压缩程序。

因为早期 xz 与 tar 没有整合，所以压缩和解压得分「两步走」。

常用选项：
* `-z,--compress`：压缩文件并删除被压缩文件
* `-d,--decompress`：解压并删除压缩包
* `-k,--keep`：压缩或解压时保留源文件
* `-0...9`：设置压缩等级，0~9，不设置默认为 6
* `-v`：压缩和解压过程详细信息

示例：

* 压缩
```shell
# 先 tar
tar -cvf xxx.tar xxx
# 压缩
xz -z xxx.tar

```
* 解压
```shell
# 解压 xz
xz -d xxx.tar.xz
# 如果保留压缩包
xz -dk xxx.tar.xz

# 解压 tar包
tar -xvf xxx.tar

```

与 tar 配合使用，通过 **-J** 参数来调用。
> 低版本的 tar 不支持，就得先 tar 再使用

示例：

```shell
# 压缩
tar -Jcf xxx.tar.xz *.jpg

# 解压
tar -Jxf xxx.tar.xz

```

---

#### <span id="linux_tarc_comptools_zip">zip</span>

zip 能通用于 Linux 和 Windows。

##### 压缩

语法：`zip(选项)(参数)`

`zip 最终压缩包名称 要打包的文件或目录`

##### 常用参数

`-r`：递归，包括了目标文件的目录一起打包
`-q`：不显示压缩过程
`-j`：不会将目标文件的目录一起打包，但子目录也同样不会打包

示例：

```shell
zip -q -r xxx ./xxx/*.txt
```

> [!tip]
> 
> 压缩包不用写后缀，最终打成包后是自动加上 `.zip` 后缀的。

##### 解压

`unzip xxx.zip`：在当前目录下解压。

`unzip xxx.zip -d ./xxx `：解压到指定目录。

`unzip -v xxx.zip`：不解压查看压缩包中的目录。

```shell
$ unzip -v SwitchWindow-0.5.0.zip 
Archive:  SwitchWindow-0.5.0.zip
649c8df77df115f9de7635553523d61874df9e1b
 Length   Method    Size  Cmpr    Date    Time   CRC-32   Name
--------  ------  ------- ---- ---------- ----- --------  ----
       0  Stored        0   0% 2023-08-08 00:51 00000000  SwitchWindow-0.5.0/
      19  Stored       19   0% 2023-08-08 00:51 a7e541a5  SwitchWindow-0.5.0/.gitattributes
      16  Stored       16   0% 2023-08-08 00:51 79d3c83b  SwitchWindow-0.5.0/.gitignore
       3  Stored        3   0% 2023-08-08 00:51 fa693369  SwitchWindow-0.5.0/.python-version
     248  Defl:N      135  46% 2023-08-08 00:51 d765ba2c  SwitchWindow-0.5.0/Default (Linux).sublime-keymap
     248  Defl:N      135  46% 2023-08-08 00:51 d765ba2c  SwitchWindow-0.5.0/Default (OSX).sublime-keymap
     248  Defl:N      135  46% 2023-08-08 00:51 d765ba2c  SwitchWindow-0.5.0/Default (Windows).sublime-keymap
     493  Defl:N      249  50% 2023-08-08 00:51 66341975  SwitchWindow-0.5.0/Default.sublime-commands
    1087  Defl:N      630  42% 2023-08-08 00:51 cf152373  SwitchWindow-0.5.0/LICENSE
     743  Defl:N      318  57% 2023-08-08 00:51 9ba993f2  SwitchWindow-0.5.0/Main.sublime-menu
    2560  Defl:N     1047  59% 2023-08-08 00:51 ffa06466  SwitchWindow-0.5.0/README.md
    4603  Defl:N     1326  71% 2023-08-08 00:51 5bba7037  SwitchWindow-0.5.0/plugin.py
--------          -------  ---                            -------
   10268             4013  61%                            12 files
```

`unzip -l xxx.zip`：列出压缩包的的文件信息，比 `unzip -v` 显示的信息更简洁一些。

```shell
$ unzip -l SwitchWindow-0.5.0.zip                   
Archive:  SwitchWindow-0.5.0.zip
649c8df77df115f9de7635553523d61874df9e1b
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  2023-08-08 00:51   SwitchWindow-0.5.0/
       19  2023-08-08 00:51   SwitchWindow-0.5.0/.gitattributes
       16  2023-08-08 00:51   SwitchWindow-0.5.0/.gitignore
        3  2023-08-08 00:51   SwitchWindow-0.5.0/.python-version
      248  2023-08-08 00:51   SwitchWindow-0.5.0/Default (Linux).sublime-keymap
      248  2023-08-08 00:51   SwitchWindow-0.5.0/Default (OSX).sublime-keymap
      248  2023-08-08 00:51   SwitchWindow-0.5.0/Default (Windows).sublime-keymap
      493  2023-08-08 00:51   SwitchWindow-0.5.0/Default.sublime-commands
     1087  2023-08-08 00:51   SwitchWindow-0.5.0/LICENSE
      743  2023-08-08 00:51   SwitchWindow-0.5.0/Main.sublime-menu
     2560  2023-08-08 00:51   SwitchWindow-0.5.0/README.md
     4603  2023-08-08 00:51   SwitchWindow-0.5.0/plugin.py
---------                     -------
    10268                     12 files
```

`unzip -j`：只保存文件名称及其内容，而不存放任何目录名称，即去掉目录，是所有目录，包括子目录，只留文件，**慎用**。

`unzip -o xxx.zip -d xx`：强制解压到 xx 目录，并覆盖原有内容。

`unzip -n xxx.zip -d xx`：解压到 xx 目录，如果目录已存在则跳过解压。

##### zipinfo

`zipinfo -s xxx.zip`：使用 `zipinfo` 能查看压缩包中的信息。不过信息的样式与 `unzip -v` 或 `unzip -l` 有所区别。

最大区别就是多了一栏 `Zip file`，显示了文件类型及读写情况，这非常适合进一步对 zip 包中目录或文件的统计操作。

```shell
$ zipinfo SwitchWindow-0.5.0.zip 
Archive:  SwitchWindow-0.5.0.zip
Zip file size: 6049 bytes, number of entries: 12
drwx---     0.0 fat        0 bx stor 23-Aug-08 00:51 SwitchWindow-0.5.0/
-rw----     0.0 fat       19 tx stor 23-Aug-08 00:51 SwitchWindow-0.5.0/.gitattributes
-rw----     0.0 fat       16 tx stor 23-Aug-08 00:51 SwitchWindow-0.5.0/.gitignore
-rw----     0.0 fat        3 tx stor 23-Aug-08 00:51 SwitchWindow-0.5.0/.python-version
-rw----     0.0 fat      248 tx defN 23-Aug-08 00:51 SwitchWindow-0.5.0/Default (Linux).sublime-keymap
-rw----     0.0 fat      248 tx defN 23-Aug-08 00:51 SwitchWindow-0.5.0/Default (OSX).sublime-keymap
-rw----     0.0 fat      248 tx defN 23-Aug-08 00:51 SwitchWindow-0.5.0/Default (Windows).sublime-keymap
-rw----     0.0 fat      493 tx defN 23-Aug-08 00:51 SwitchWindow-0.5.0/Default.sublime-commands
-rw----     0.0 fat     1087 tx defN 23-Aug-08 00:51 SwitchWindow-0.5.0/LICENSE
-rw----     0.0 fat      743 tx defN 23-Aug-08 00:51 SwitchWindow-0.5.0/Main.sublime-menu
-rw----     0.0 fat     2560 tx defN 23-Aug-08 00:51 SwitchWindow-0.5.0/README.md
-rw----     0.0 fat     4603 tx defN 23-Aug-08 00:51 SwitchWindow-0.5.0/plugin.py
12 files, 10268 bytes uncompressed, 4013 bytes compressed:  60.9%

```

zipinfo 也有短格式：

```shell
$ zipinfo -s zip_tmp.zip
Archive:  zip_tmp.zip
Zip file size: 9868321 bytes, number of entries: 17
drwxr-xr-x  2.0 unx        0 bx stor 24-Sep-18 23:18 tmp/
-rw-r--r--  2.0 unx  1305313 bX defN 24-Jun-16 23:42 tmp/A File Icon.sublime-package
-rw-r--r--  2.0 unx   655376 bX defN 24-Sep-18 21:15 tmp/CommandsBrowser.sublime-package
-rw-r--r--  2.0 unx   200864 bX defN 24-Sep-18 21:14 tmp/ConvertToUTF8.sublime-package
-rw-r--r--  2.0 unx   132169 bX defN 21-Dec-07 14:44 tmp/Emmet.sublime-package
-rw-r--r--  2.0 unx  3623354 bX defN 24-Sep-18 21:14 tmp/gruvbox.sublime-package
-rw-r--r--  2.0 unx    99791 bX defN 24-Sep-18 21:15 tmp/Less.sublime-package
-rw-r--r--  2.0 unx  3884049 bX defN 24-Sep-18 21:14 tmp/NeoVintageous.sublime-package
-rw-r--r--  2.0 unx    14464 bX defN 24-Sep-18 21:14 tmp/NeoVintageousHighlightLine.sublime-package
-rw-r--r--  2.0 unx   306208 bX defN 24-Sep-18 21:14 tmp/SideBarTools.sublime-package
-rw-r--r--  2.0 unx   228092 bX defN 24-Feb-14 03:52 tmp/ST4_DoxyDoxygen.sublime-package
-rw-r--r--  2.0 unx     5563 bX defN 23-Aug-08 03:56 tmp/Switch Window.sublime-package
-rw-r--r--  2.0 unx    12366 bX defN 24-Sep-18 21:15 tmp/Terminal.sublime-package
-rw-r--r--  2.0 unx   116812 bX defN 24-Sep-18 21:15 tmp/Terminus.sublime-package
drwxr-xr-x  2.0 unx        0 bx stor 24-Sep-19 10:42 zip_tmp/
-rwxr-xr-x  2.0 unx      416 bX defN 24-Feb-29 11:15 4169_cracked_untested.sh
-rwxr-xr-x  2.0 unx      623 bX defN 24-Sep-14 03:37 4180_cracked_untested.sh
17 files, 10585460 bytes uncompressed, 9864875 bytes compressed:  6.8%

```

配合 Linux 其他命令，可以作进一步信息的查看，如查看目录情况：

```shell
$ zipinfo AFileIcon-3.28.0.zip | grep "^d"        
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/core/
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/core/utils/
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/core/vendor/
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/icons/
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/icons/multi/
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/icons/single/
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/media/
drwx---     0.0 fat        0 bx stor 24-Jun-16 23:38 AFileIcon-3.28.0/preferences/
```

只显示文件名：

```shell
$ zipinfo -1 zip_tmp.zip
tmp/
tmp/A File Icon.sublime-package
tmp/CommandsBrowser.sublime-package
tmp/ConvertToUTF8.sublime-package
tmp/Emmet.sublime-package
tmp/gruvbox.sublime-package
tmp/Less.sublime-package
tmp/NeoVintageous.sublime-package
tmp/NeoVintageousHighlightLine.sublime-package
tmp/SideBarTools.sublime-package
tmp/ST4_DoxyDoxygen.sublime-package
tmp/Switch Window.sublime-package
tmp/Terminal.sublime-package
tmp/Terminus.sublime-package
zip_tmp/
4169_cracked_untested.sh
4180_cracked_untested.sh

```

###### zipinfo 示例

 1. 统计包中目录的数量

```shell
zipinfo xxx.zip | grep "^d" | wc -l
```

2. 列出包中一级文件（包括目录）

```shell
zipinfo -1 zip_tmp.zip | awk 'BEGIN{FS="/"}{print $1}' | uniq
```

或者写成这样：

```shell
zipinfo -1 AFileIcon-3.28.0.zip | awk -F "/" '{print $1}' | uniq
```

> [!info] 
> 
> `awk 'BEGIN{FS="/"}'` 这是改变分隔符为斜杠 `/`，`{print $1}` 取出第一列，就是一级文件。
> 
> `uniq` 很简单去重，把相同一级目录都去掉重复，就知道 zip 包中一级文件与目录有多少了。

---

#### <span id="linux_tarc_comptools_rar">rar</span>

在 Linux 系统中压缩和解压 `.rar` 格式的压缩包，得使用专门的程序。

在 Linux 系统 `.rar` 不是常用压缩格式，所以一般更多的是解压，而在 Linux 系统中解压 `.rar` 压缩包得选安装 [unrar](https://www.rarlab.com/download.htm) 程序。

```shell

# Ubuntu
# 安装 rar
sudo apt install rar

# 安装 unrar
sudo apt install unrar

```

```shell

# ArchLinux、manjaro
# 安装 rar
pacman -S rar

# 安装 unrar
pacman -S unrar


```

> [!tip]
> 
> [ArchLinux_Note](ArchLinux_Note.md) 如果安装了 [AUR Helper](ArchLinux_Note.md#AUR%20Helper)，如 [yay](ArchLinux_Note.md#yay)，就能使用相应的 aur helper 来安装。

`unrar` 简单示例：

```shell

# 不解压只查看压缩包中的信息
# 简略模式
unrar l task.rar
# 详细模式
unrar v task.rar

# 解压
unrar e a.rar





```

`rar` 示例：

```shell
# rar a -vSIZE  压缩后的文件名 被压缩的文件或者文件夹
# -v后跟的是分包的大小，单位可以是b/B、k/K、m/M、g/G、t/T。

rar a -v50000k eclipse.rar eclipse

# 解压
# 如果压缩包是以分包形式存在，只要指定第一个分包，程序会自动进行下一个分包的解压
rar x xxx.part1.rar

```
> [!quote] 
> 
> * [分卷压缩及解压分卷压缩文件 - Ubuntu中文](https://wiki.ubuntu.org.cn/%E5%88%86%E5%8D%B7%E5%8E%8B%E7%BC%A9%E5%8F%8A%E8%A7%A3%E5%8E%8B%E5%88%86%E5%8D%B7%E5%8E%8B%E7%BC%A9%E6%96%87%E4%BB%B6)

#### 其他压缩小技巧

###### zip 包分包及并包

分包
```shell
# 先压成zip包
zip -r temp.zip data/
# 然后对这个zip包进行分包操作
zip -s 10m temp.zip --out data.zip
```

> [!tip] `zip` 命令
> 
> `-s` 是指定分包的大小。`10m`、`10g` 等
> 
> `--out` 指定分包的包名

将分包合并再解压：

方法 1：
```shell
# 先将分包合并成一个包
cat data.* > tounzip.zip
# 然后再解压
unzip tounzip.zip
```

> [!tip]
> 并包时使用到了 `>` 这个重定向符
> 此方法我试过，并包存在错误。

方法 2：
```shell
zip -F 烤肉500.zip --out 500.zip
```
> [!tip]
> `-F` 是修复压缩包
> 
> 这种方式就是通过修复包的方式合并压缩包。进过测试，这方法是有效的。
> 

相关连接：
* [Linux zip分卷压缩解压-CSDN博客](https://blog.csdn.net/qq_19320227/article/details/127967543)
* [linux zip分卷压缩和解压缩 - 知乎](https://zhuanlan.zhihu.com/p/634435955)

---

## <span id="linux_file">文件</span>

### <span id="linux_file_search">文件查找</span>

#### find

#### fd

[fd](https://github.com/sharkdp/fd) 是一个更友好的 [find](#find)，它是用 [Rust](../Rust/Rust_Note.md) 编写。

---

## <span id="linux_textprocessing"> 文本处理 </span>

### <span id="linux_textprocessing_commands"> 文本处理常用命令 </span>

#### <span id="linux_textprocessing_commands_cat">cat</span>

`cat` 命令是由第一行开始显示文件内容。

常用选项及参数：
* `-A`：可列出一些特殊字符而不是空白而已
* `-b`：列出行号，仅针对非空白行做行号显示，空白行不标行号
* `-E`：结尾的断行字符 **$** 显示出来
* `-n`：打印出行号，连空白行也会有行行号
* `-T`：将 **tab** 键以 **\^I** 显示出来
* `-v`：列出一些看不出来的特殊字符

`cat -n` 这是将行号也一并打印出来。

![cat n](./Linux_Note.assets/linux_textprocessing_cat_n.png)

##### cat 常用示例

```shell

# 将某文件内容清空，但不删除
cat /dev/null > t01.txt

```

#### <span id="linux_textprocessing_commands_tac">tac</span>

`tac` 命令是从最后一行开始显示，`tac` 是 `cat` 的倒着写。

#### <span id="linux_textprocessing_commands_nl">nl</span>

`nl` 添加行号打印

`nl` 选项和参数：

| 选项和参数 | 功能 |
| :---: | :---: |
| -b, --body-numbering=样式    |    使用 <样式> 对正文的行进行编号 |
 |  -d, --section-delimiter=CC |      使用 CC 作为逻辑页分隔符 |
 | -f, --footer-numbering=样式  |   使用 <样式> 对页脚的行进行编号 |
 | -h, --header-numbering=样式  |   使用 <样式> 对页眉的行进行编号 |
 | -i, --line-increment=数值    |   设置每一行行号的自动递增值 |
 | -l, --join-blank-lines=数值  |   将 <数值> 行连续的空行视为一行 |
 | -n, --number-format=格式     |   按照指定 <格式> 插入行号 |
 | -p, --no-renumber            |   在切换至下一节时不重置行号值 |
 | -s, --number-separator=字符串   | 在可能出现的行号后添加 <字符串> |
 | -v, --starting-line-number=数值 |   每一节第一行的行号 |
 | -w, --number-width=数值 |       设置行号的宽度为 <数值> 列 |

样式列表：

| 样式参数 | 功能 |
| :---: | :---: |
| a |  对所有行编号 |
| t | 仅对非空行编号 |
| n |  不对任何行编号 |
| pBRE | 仅对匹配基本正则表达式 BRE 的行编号 |

示例：

```shell
# 默认什么参数都不加
nl Ship_Note.md
```
![nl 1](./Linux_Note.assets/linux_textprocessing_nl_1.png)

```shell
# 只有非空行显示行号
nl -b t Ship_Note.md
```

```shell
# 所有行都不显示行号
nl -b n Ship_Note.md
```

```shell
# 所有行都显示行号，包括空行
nl -b a Ship_Note.md
```

行号格式：

| 参数 | 对齐方式 | 有无前导 0 |
|:---: |:---: |:---: |
| ln | 左对齐 | 无前导 0 |
| rn | 右对齐 | 无前导 0 |
| rz | 右对齐 | 有前导 0 |

示例：
```shell
nl -b t -n ln Ship_Note.md
```
![nl ln](./Linux_Note.assets/linux_textprocessing_nl_ln.png)

```shell
nl -b t -n rn Ship_Note.md
```
![nl rn](./Linux_Note.assets/linux_textprocessing_nl_rn.png)

```shell
nl -b t -n rz Ship_Note.md
```
![nl rz](./Linux_Note.assets/linux_textprocessing_nl_rz.png)

效果相似命令对比：

与 `nl` 命令相似，但 `cat -n` 是连空行也一并加上行号，而 `nl` 默认不加任何参数或选项，行号是忽略空行。如下图：

![cat n diff nl](./Linux_Note.assets/linux_textprocessing_cat_n_diff_nl.png)

#### <span id="linux_textprocessing_commands_more">more</span>

[cat](#linux_textprocessing_commands_cat)、[tac](#linux_textprocessing_commands_tac) 和 [nl](#linux_textprocessing_commands_nl) 这三个命令都是把文件一次性打印在屏幕上。想要一页页的翻动查看，就使用 `more` 命令和 [less](#linux_textprocessing_commands_less) 命令。

more 命令一些按键操作：
* 空格（space）： 向下翻一页
* Enter： 向下一行
* `/`： 向下搜索「字符串」
* `:f`： 显示出文件名及目前显示的行数
* q： 退出 more，不再显示该文件内容
* b 或 ctrl-b： 往回翻页，只对文件有用

`more` 命令中的搜索：
`/` 后输入要搜索的内容，按回车就开始向下搜索，按 `n` 就会跳转到下一个搜索结果。

#### <span id="linux_textprocessing_commands_less">less</span>

`less` 命令比 [more](#more) 命令要灵活很多，`more` 命令只能向下翻页，而 `less` 可上可下。

参数：

* `-N`：显示行号

#### <span id="linux_textprocessing_commands_head">head</span>

#### <span id="linux_textprocessing_commands_tail">tail</span>

#### <span id="linux_textprocessing_commands_tr">tr</span>

#### <span id="linux_textprocessing_commands_wc">wc</span>

`wc` 命令用于统计指定文本的行数、字数、字节数，格式：`wc [参数] 文本`。

| 参数 | 功能 |
| :---: | :---: |
|  -l    |  统计行数     |
|  -w   |    统计字数  |
|   -c   |     统计字节数 |

如果任何参数都不给，就是依次显示行数、字数和字节数的统计结果。

#### <span id="linux_textprocessing_commands_stat">stat</span>

`stat` 命令用于查看文件的具体不住信息和埋单信息，格式为：`stat 文件名称`。

### <span id="linux_textprocessing_regex"> 正则表达式 </span>

### <span id="linux_textprocessing_grep">grep</span>

 #grep

[Grep 笔记](Grep_Note.md)

### <span id="linux_textprocessing_sed">sed</span>

 #sed 

### <span id="linux_textprocessing_awk">awk</span>

 #awk 
 
### awk 历史与简介

awk 写于 1977 年。作者：Alfred V *A*ho、Peter J.*W*einberger 和 Brian W *K*ernighan。awk 这名，就是取自三位作者姓氏的首字母。

awk 有多处版本，包括旧版的 awk、nawk（new awk）、gawk（GNU 版本）以及 POSIX awk 等。

现在大部分 Linux 发行版默认装的都是 gawk，而 awk 只是 gawk 的一个软链接 -- 这在 Red hat Linux9 时代就已经是这样了。

### 语法

awk 逐行扫描文件，从第一行到最后一行寻找匹配对家模式的行，并在这些行上运行指定的动作。

awk 由包含**模式**和**动作**的单行或多行文本组成。

awk 语法结构：`awk 'pattern {action} pattern {action}..' 文件`

#### 列

* `$0`：表示整行文本。
* `$1`：表示文本行中的第一列。
* `$N`：表示文本行中第 N 列。
* `$NF`：表示文件行中的最后一列。

#### 行

`NR==N`：指定行号

##### 多行

使用逗号 `,` 分隔，表示显示一个连续范围的行，即标示要打印的「区间」，如 `NR==1,NR==3`：打印 1 至 3 行

使用分号 `;` 分隔，表示分别显示各行，如 `NR==1;NR==3;NR==6`：打印第 1 行、第 3 行和第 6 行。

###### 示例

行和列综合使用：

`unzip -l xxx.zip |awk 'NR==5{print $NF}'`：列出 zip 包中的文件列表，显示第 5 行最后一列信息

---

## <span id="linux_font"> 字体 </span>

安装 [字体](../Fonts/Fonts_Note.md)：

1. 使用字体安装程序点击安装
> 也可以直接将字体文件复制到 `/usr/share/fonts` 目录下  
> 如果是 ttf 字体，应复制在 `/usr/share/fonts/TTF` 目录下  
> 也可以直接把字体文件复制到 `~/.local/share/fonts` 目录， 这是仅当前用户可用的字体

2. 刷新
```shell
fc-cache -fv
```

查看字体：
```shell
# 查看中文字体
fc-list :lang=zh
```

---

## <span id="linux_network"> 网络 </span>

### <span id="linux_networkd"> 一些网络概念 </span>

相关笔记列表：

* [Network_Note](../Network/Network_Note.md)
* [Http_Note](../Network/Http_Note.md)

#### 以太

**以太** 是一种虚拟的物质，英文 Ether 的音译，一般可以理解为一种看不见摸不着的物质。

最早提出「以太」概念是亚里士多德。

随着科学发展，「以太」理论也逐渐被科学界抛弃。

虽然以太理论已经基本被淘汰，但「以太」这词在使用中并未消亡，「以太网」就是其一。

以太网是一种计算机局域网技术，而 IEEE 802.3 标准则是以太网技术被广泛应用之后制定的以太网技术标准。

以太网是目前应用最普遍的局域网技术。

也就是因为有此原因，所以 Linux 系统中网络接口都使用 「eth」为名称。

#### <span id="linux_network_bridge"> 网桥 </span>

简单说，网桥就是连通两个本来处于隔离的网络的一个「桥梁」。 

---

### <span id="linux_network_command">Linux 中网络相关命令和工具 </span>

#### <span id="linux_network_command_ip">IP 概念 </span>

`ip` 命令 是用来查询或维护网络相关的工具。

包括 **路由**（Routing）、**网络设备**（Device）、**策略路由**（Policy Routing）以及 **隧道**（Tunnel）。

##### ip 常用命令

* `ip link`：显示网络设备
* `ip addr` ：查看网络地址  这个命令是把网络设置及网络地址、掩码都显示出来。比 `ip link` 多显示一些信息。
* `ip linke show 网络设备名称`：显示指定网络设备的信息。
* `ip link set 网络设备 up`：启用网络设备
* `ip link set 网络设备 down`：禁用网络设备

#### <span id="linux_network_command_downloader"> 命令下载工具 </span>

##### <span id="linux_network_command_downloader_curl">curl</span>

[curl](https://curl.se) [![curl repo](https://img.shields.io/github/stars/curl/curl?style=social)](https://github.com/curl/curl) 是一个用来对服务器发起请求并把响应输出到标准输出的工具。所以单纯把它当成一个下载器，有点小瞧它了。

###### curl 常用操作

```shell
# 什么参数选项都不要，就是向指定地址发送一个 get 请求
# 然后将服务器响应的内容打印在屏幕上
curl http://www.baidu.com
```

```shell
# 包括协议的响应的 head 信息
# Include protocol response headers in the output
curl -i, --include url

# Show document info only
curl -I, --head url

# 返回状态码
curl -w "%{http_code}\n" url

# 使用 /dev/null 把多余的信息过滤掉
curl -w "%{http_code}\n" -o /dev/null url

# 只返回状态码
curl -s -o /dev/null -w "%{http_code}\n" url

```

```shell
# 将服务器响应保存成文件`a.html`
curl -o a.html http://xxx.com
```

```shell
# 注意这是 大 O，与上面的小 o 需要自已定义保存文件名不一样
# 将服务器响应保存成文件，以请求的最后部分当成文件名
curl -O http://www.com/a.html
```

```shell
# 指定代理
curl -x [protocol://]host[:port] Use this proxy
```

```shell
# 指定请求方式 : GET POST 等
# 一般来说，请求 GET 和 POST 都能向服务器发出获取数据的请求
# 但 HTTP 协议在制定时，默认给了这些方式相应的语义，从这个请求的名称就能看出
# GET 请求是向服务器发出获取数据的请求；而 POST 请求，是将数据提交到服务器的请求
# 示例
curl -X POST
```

```shell
# 指定参数，或提交数据
curl -d 
```

> [!info] 相关资料
> 
> * [curl 的用法指南 - 知乎](https://zhuanlan.zhihu.com/p/336945420)
> * [cURL命令详解 - 知乎](https://zhuanlan.zhihu.com/p/661602561)
> * [通过curl获取HTTP状态返回码](https://blog.csdn.net/weixin_46686835/article/details/113761418)
> * [cURL 如何只返回状态码](https://www.cnblogs.com/lsgxeva/p/13915259.html)
> * [curl/wget 测试http请求的响应头信息 - dorothychai - 博客园](https://www.cnblogs.com/dorothychai/p/4381931.html)
> * [curl 命令详解，省的来回找了【Linux】-腾讯云开发者社区-腾讯云](https://cloud.tencent.com/developer/article/1747958)

##### <span id="linux_network_command_downloader_wget">wget</span>

 [wget](https://www.gnu.org/software/wget/) 跟 [curl](https://curl.se) 类似的命令行下载器。

 wget 与 [curl](#linux_network_command_curl) 不一样，它主要就是用于下载。

###### wget 主要操作

```shell
# 无参 直接下载文件
wget http://xxx.com
```
  由无参这种 **默认** 方式，wget 是直接下载文件，就能看出 wget 重点是放在下载上的。

1. 重命名

`-o` 或 `-O` 是重命名下载文件的文件名。

> [!tip] 
> 
> 这操作实际已经包含指定目录的操作，即包含了 `-P` 的操作，如果重命名时，目录不存在，会像 `-P` 一样先创建目录，然后才开始下载然后对下载文件进行重命名。

 ```shell
 # 大O 下载并重命令文件
 # 相当于 curl 的小o参数的操作
 # 如果当前目录已存在同名文件将覆盖
 
 wget -O rename.zip http://xxxx.com/xxx.xxx
 
 # 小o与大O类似，就是重命名文件
 # 区别是小o不显示下载过程
 wget -o rename.zip http://xxxx.com/xxx.xxx
```

2. 指定目录

`-P` 操作非常单纯，就是指定要下载到哪个目录。

如果只是想指定下载目录，可以使用这个参数，但如果同时想进行重命名，那就应该使用 `-o` 或 `-O` 参数。

```shell
# 指定下载到哪目录
# 文件名使用下载地址最后部分
# -P 指定的目录如果不存在则自动创建
wget -P /home/test http://xxxx
```

3. 断点续传

```shell
# 断点下续传
# 使用 wget -c 重新启动下载中断的文件
wget -c http://xxx.com
```

 `-c` 和 `-O` 组合使用下载示例：
 
 ```shell
 wget -c -O miniconda_py39_4.12.0.sh https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py39_4.12.0-Linux-x86_64.sh
```

4. 限速

```shell
# --limit-rate= 使用来限速的
wget --limit-rate=400k URL
```

5. 检测连接

```shell
# 检测连接
wget –-spider URL
```

6. 关闭详细输出信息

```shell
# 关闭详尽输出，但不进入安静模式
wget -nv, --no-verbose                
```

示例：

![wget spider](./Linux_Note.assets/wget_spider.png)

```shell
# 用于下载一个网站的多个资源
# -r 是递归下载
wget -r URL
# 与递归下载配合使用，限定只下载指定类型的文件
# -A 或 --accept= 后跟限定的文件类型，即文件扩展名列表，名扩展名使用逗号分隔
wget -r -A 扩展名 URL

# 与 -A 或 --accept 相反，--reject= 是拒绝下载指定类型文件，后面同样跟的是扩展名列表，同样使用逗号分隔
wget -r --reject=pdf,exe URL

# -l 或 --level=数字 指定递归深度，0 为不限制
wget -r -l 1 URL

# 示例：
# 只递归一层 只下载 扩展名为 azw3 的文件
wget -r -l 1 -A azw3 http://xxxxx

```

wget 相关资料：

* [Linux系统中10个使用Wget命令下载文件示例 - 知乎](https://zhuanlan.zhihu.com/p/98778885)
* [Linux必备下载命令之wget详解 - 知乎](https://zhuanlan.zhihu.com/p/336487919)
* [wget下载文件到指定目录](https://blog.csdn.net/willingtolove/article/details/108802004)

##### <span id="linux_network_command_downloader_aria2">aria2</span>

[aria2](http://aria2.github.io)[![aria2 repo](https://img.shields.io/github/stars/aria2/aria2?style=social)](https://github.com/aria2/aria2) 是一款非常强大的下载工具。

看它的一长串的定语，就知道它的强大：「多平台」、「支持多协议」、「支持多来尖」。

aria2 可以从多个来源、多个协议下载资源，最大程序上利用了你的带宽。

aria2 有着非常小的资源占用，在关闭磁盘缓存的情况下，物理内存占用通常为 4M，BT 下载，CPU 占有率约为 6%。

aria2 特点：
* 高速、自动多线程下载；断点续传
* 内存占用少
* 多平台，支持 Windows、Linux、OSX、Android 等操作系统
* 模块化；分段下载引擎，文件整合速度快
* 支持 PRC 界面远程
* 全面支持 BtiTorrent 协议

##### <span id="linux_network_command_downloader_aria2_operate">aria2 常用操作 </span>

最简单的使用，就是什么参数或选项都不加：
```shell
# 直接在命令行下载，下载完成自动退出，跟 wget 的工作方式类似
aria2c URL
```

设置连接数量：
```shell
# 下载连接的数量 默认为 5
-s, --split=N 
```

断点续传：
```shell
# 继续下载一个仅部分分完成的文件
-c, --continue[=true|false] 
```

指定文件保存的文件名：
```shell
-o, --out=FILE
```

后台下载：
```shell
-D,–-deamon=true
```

下载速度限制：
```shell
# 单文件下载速度限制 
# 单位 K 或 M
--max-download-limit=速度
# 全局
# 单位 K 或 M
--max-overall-download-limit=速度
```

```shell
# 0 意味着不限制。
# 可附加 K 或 M（1K=1024，1M=1024K）
-u, --max-upload-limit=速度
```

文件预分配方式：
```shell
# 文件预分配方式, 可选：none, prealloc, trunc, falloc, 默认:prealloc
# 预分配对于机械硬盘可有效降低磁盘碎片、提升磁盘读写性能、延长磁盘寿命。
# 机械硬盘使用 ext4（具有扩展支持），btrfs，xfs 或 NTFS（仅 MinGW 编译版本）等文件系统建议设置为 falloc
# 若无法下载，提示 fallocate failed.cause：Operation not supported 则说明不支持，请设置为 none
# prealloc 分配速度慢, trunc 无实际作用，不推荐使用。
# 固态硬盘不需要预分配，只建议设置为 none ，否则可能会导致双倍文件大小的数据写入，从而影响寿命。
--file-allocation=方式
```

列出 BT 种子的内容：
```shell
aria2c -S **.torrent
```

开启 RPC：
```shell
aria2c --enable-rpc --rpc-listen-all
```

###### <span id="linux_network_command_downloader_aria2_config">Aria2 配置 </span>

默认用户配置文件放在：`~/.aria2/aria2.conf`
也可以通过命令行 `aria2 --conf-path` 命令指定配置文件路径。
示例：
```shell
aria2c --conf-path="/etc/aria2/aria2.conf"
```

Aria2 除了使用命令行临时配置，还可以将配置写进配置文件 `aria2.conf`。

常用配置：
```
# 磁盘缓存, 0 为禁用缓存，默认:16M
# 磁盘缓存的作用是把下载的数据块临时存储在内存中，然后集中写入硬盘，以减少磁盘 I/O ，提升读写性能，延长硬盘寿命。
# 建议在有足够的内存空闲情况下适当增加，但不要超过剩余可用内存空间大小。
# 此项值仅决定上限，实际对内存的占用取决于网速(带宽)和设备性能等其它因素。
disk-cache=64M

# 文件预分配方式, 可选：none, prealloc, trunc, falloc, 默认:prealloc
# 预分配对于机械硬盘可有效降低磁盘碎片、提升磁盘读写性能、延长磁盘寿命。
# 机械硬盘使用 ext4（具有扩展支持），btrfs，xfs 或 NTFS（仅 MinGW 编译版本）等文件系统建议设置为 falloc
# 若无法下载，提示 fallocate failed.cause：Operation not supported 则说明不支持，请设置为 none
# prealloc 分配速度慢, trunc 无实际作用，不推荐使用。
# 固态硬盘不需要预分配，只建议设置为 none ，否则可能会导致双倍文件大小的数据写入，从而影响寿命。
file-allocation=none

# 文件预分配大小限制。小于此选项值大小的文件不预分配空间，单位 K 或 M，默认：5M
no-file-allocation-limit=64M

# 断点续传
continue=true

# 文件预分配大小限制。小于此选项值大小的文件不预分配空间
# 单位 K 或 M，默认：5M
no-file-allocation-limit=64M

# GZip 支持，默认:false
http-accept-gzip=true

```

网上大神配置模板，可以参考着配：[https://github.com/P3TERX/aria2.conf/blob/master/aria2.conf](https://github.com/P3TERX/aria2.conf/blob/master/aria2.conf)

###### <span id="linux_network_command_downloader_aria2_ui">Aria2 UI 工具 </span>

[aria2](#linux_network_command_downloader_aria2) 本身没有 UI ，这使得一些依赖图形界面的用户用起来不太方便，然后就出现各种基于 aria2 的带图形界面的下载器。 

下面介绍几款，用得比较多的基于 aria2 的下载器。

**Motrix**

[Motrix](https://motrix.app) 是一款开源的支持多平台的，基于 [aria2](#linux_network_command_downloader_aria2) 的下载工具。

它支持 HTTP、FTP、BT、磁力链、百度网盘等资源，界面简洁，开箱即用。

**AriaNg**
[AiraNg](https://github.com/mayswind/AriaNg) 基于 Aria2 Web 前端。同样也是开源的。

![AriaNg screenshot](https://raw.githubusercontent.com/mayswind/AriaNg-WebSite/master/screenshots/desktop.png)

---

### IPtables 

IPtables 中可以做各种网络地址转换，网络地址转换主要有两种：**SNAT** 和 **DNAT**。

**SNAT** 是 「source networkaddress translation」的缩写，即「源地址目标转换」。

**DNAT** 是 「destination networkaddress translation」的缩写，即「目标网络地址转换」。

### 代理

#### 临时代理

 #proxy

想要在当前窗口临时设下代理，可以使用 `export` 命令设置 `http_proxy` 或 `https_proxy` 的值。如下：

```shell
export http_proxy=http://127.0.0.1:端口
# 或者
export https_proxy=http://127.0.0.1:端口

# 当然可以二个一起设
export all_proxy=socks5://127.0.0.1:端口
```

如果使用 socks5 就这样设置：

```shell
export http_proxy=socks5://127.0.0.1:端口
# 或者
export https_proxy=socks5://127.0.0.1:端口
# 同样的可以二个一起设
export all_proxy=socks5://127.0.0.1:端口
```

> [!tip] 
> 
> `http_proxy` 后面的值，可以加双引号 `"`，也可以不加。即可以写成这样：`export https_proxy="http://127.0.0.1:7897"`
> 
> 如果使用 Clash 客户端，如 [Clash-Verge-Rev](../Ladder/Ladder_Note.md#Clash-Verge-Rev)，其中有个功能能方便复制「环境变量」，点一个，就能复制出要执行的代码，诸如：`export https_proxy=http://127.0.0.1:7897 http_proxy=http://127.0.0.1:7897 all_proxy=socks5://127.0.0.1:7897`，将代码贴到终端中，执行下，当前终端窗口就能启用临时代理了。
> 

设置完，先用 `echo $http_proxy` 命令查看当前窗口是否设置了 `http_proxy` 值。然后测试下代理是否真能生效：

1. `curl ip.gs`
2. `curl cip.cc`
3. `curl https://www.google.com`

清除环境变量可以这样：

```shell
unset http_proxy  
unset https_proxy
```

> [!info] 相关资料
> 
> * [linux终端设置临时代理 - 建站笔记 - 博客园](https://www.cnblogs.com/dede369/p/14475170.html)
> * [Windows / Linux 下为 命令行 设置临时代理](https://blog.csdn.net/weixin_45956258/article/details/120423310)
> * [linux命令配置代理 • Worktile社区](https://worktile.com/kb/ask/313574.html)
> * [终端使用代理加速的正确方式（Clash） | Ln's Blog](https://weilining.github.io/294.html)

---

## <span id="linux_ps">进程</span>

### 常用命令
* `ps aux`
* `ps -ef`
* `ps -elf`

`PID`：进程 ID
`PPID`：父进程 ID

大部分应用程序的父进程都是 `/usr/lib/systemd/systemd --user` 这个当前用户主进程。

### 常用示例

#### 按进程名称查询

`ps -ef |grep '进程名'`：按进程名称搜索正在运行的进程
> [!tip]
> 这个查询的结果会多一个， 这个进程是 `grep` 的。
>
> 过滤掉 `grep` 本身进程：
> `ps -elf |grep '进程名' | grep -v grep`：这是使用 `grep -v` 把 grep 本身的进程过滤掉，这才是真正想要的结果。
>

* `pgrep '进程名' -a`：显示进程 PID 及进程的程序全路径名
* `pgrep '进程名' -l`：显示进程 PID 及进程名，比上面的简洁一些

---

## <span id="linux_ssh">SSH</span>

### <span id="linux_ssh_insconf"> 安装和设置 SSH</span>

以 debian 系为例

#### 安装：

```shell
apt-get update
apt-get upgrade

apt-get install openssh-server
```

#### 设置：

对 `/etc/ssh/sshd_config` 文件进行设置。

* 开启端口：`Port 22`

* 监听地址：`ListenAddress 0.0.0.0`

* 添加 `PermitRootLogin yes` 使用 root 登录的权限

* 将 `PermitTTY` 设为 `yes`

* 将 `UsePAM` 设为 `no`

#### 重启

```shell
service ssh restart
# 或者
systemctl restart sshd.service

```

> [!note] 
> 
> 查看 ssh 是否已开启，可以使用 `ssh -V` 命令。
> 
> 或者使用 `systemctl status sshd.service` 来查看 ssh 服务状态。

#### 连接

使用 ssh 命令连接到远程服务器：

```shell
ssh 用户名@ip -p 端口
```

### <span id="linux_ssh_bugs">SSH 各种问题 </span>

`Host key verification failed` 错误

解决方法：

方法 1： 直接将 `.ssh/known_hosts` 文件删除即可。

方法 2： 修改 `/etc/ssh/ssh_config` 配置文件中，设置 `StrictHostKeyChecking` 的值

默认情况下，`StrictHostKeyChecking=ask` 。出现提示，如果连接和 key 不匹配，给出提示，并拒绝登录。

`StrictHostKeyChecking=no` 是最不安全的级别。如果连接 server 的 key 在本地不存在，那么就自动添加到文件中（默认是 `.ssh/known_hosts` 文件中），并且给出一个警告。

`StrictHostKeyChecking=yes` 是最安全级别。如果连接与 key 不匹配，就拒绝连接，不会提示详细信息。

### <span id="linux_ssh_tools">SSH 工具 </span>

#### <span id="linux_ssh_tools_putty">Putty</span>

这是款古老的 SSH 工具。

优点：小巧、免费。
缺点：设置不太人性化

#### <span id="linux_ssh_tools_electerm">Electerm</span>

[Electerm](https://electerm.html5beta.com) [![Electerm Repo](https://img.shields.io/github/stars/electerm/electerm?style=social)](https://github.com/electerm/electerm) 开源免费，而且 win、mac 和 linux 平台都能用。

其实 [Electerm](https://electerm.html5beta.com) 是能当终端使用的。

颜值还非常高：
![electerm](https://github.com/electerm/electerm-resource/raw/master/static/images/electerm.gif)

#### <span id="linux_ssh_tools_tabby">Tabby</span>

[Tabby](https://tabby.sh) [![Tabby Repo](https://img.shields.io/github/stars/Eugeny/tabby?style=social)](https://github.com/Eugeny/tabby) 同样是一款 Win、Linux 和 Mac 都能用的开源 SSH 工具。之前的名字叫 「Terminus」。

其实这货更像是一个终端，只是集成了 SSH 功能。

这颜值比 [Electerm](https://electerm.html5beta.com) 更高，功能更强。就是字体设置不太方便，还得手动输入字体名。

这货还能装插件，太过分了！

有一大堆的配色方案可选，真的非常过分了！

搭配插件，tabby 的配置能使用 gist 进行同步。

#### <span id="linux_ssh_tools_finalshell">FinalShell</span>

[FinalShell](https://www.hostbuf.com) 是一款 Win、Linux 和 Mac 都能用的 ssh 工具。

优点：功能强、设置顺手，所有平台都能用

缺点：耗内存、启动有点慢

#### terminal 相关

##### 复制粘贴

在通常情况下，粘贴剪切版的内容使用 `Shift+Insert` 就可以实现。

但在终端中，`Shift+Insert` 只能粘贴第一次复制的内容，如果有新复制的内容，使用 `Shift+Insert` 就会失效。

解决这个问题其实很简单，只要使用 `Ctrl+Shift+Insert` 就好了！

> [!info] 相关资料
> 
> * [Linux剪贴板shift+insert无效解决方案](https://blog.csdn.net/tcliuwenwen/article/details/106988784)

---

## <span id="linux_shell">Shell 相关 </span>

### sh 命令

`sh` 命令其功能是 [Shell](Shell/Shell_Note.md) 语言的解释器。
>[!tip] 
>
> `sh` 其实不是实际存在的命令文件，而是 `bash` 的别名命名，所以使用 `bash` 效果相同。

语法格式：`sh [参数] 脚本名`

#### 参数

* `-c`：从字符串中读取命令，即可以执行简单的命令
> [!example] 
> ```shell
> sh -c 'echo "hello world!"'
> ```
>
> 
* `-i`：实现脚本交互
* `-n`：进行语法检查
* `-v`：显示执行过程详细信息
* `-x`：实现逐条语句跟踪
* `--help`：帮助文档
* `--versoin`：sh 的版本信息

Shell 语言相关内容：[Shell笔记](Shell/Shell_Note.md)

---

## <span id="linux_soft_installsetup"> 部分软件安装设置 </span>

---

### <span id="linux_soft_install_desktop"> 使用 desktop</span>

1. 新建 `desktop` 文件
在/usr/share/applications/目录中新建一个后缀名为 desktop 的文件
> [!tip] 
> 
> 也可以在 .local/share/applications 目录新建 desktop 文件

2. 编辑 `desktop` 文件
> [!info] desktop 文件格式
>  
> [Desktop Entry] (这里大小写敏感，写错一个就不会显示)  
> Type=Application 类型（这个属性是必须的，默认可以就写 Application）
> Name=名称  
> Name[zh_CN]=简中名称  
> Name[zh_TW]=繁中名称  
> Comment=鼠标放在图标上时的提示文字  
> Comment[zh_CN]=与上类似不过是简体中文  
> Comment[zh_TW]=繁中的提示文字  
> Icon=图标路径  
> Exec=可执行文件的路径  
> Categories=程序的分类标签，多个标签使用; 号分隔 (Application;IDE;Development;Editor)  
> Terminal=false 是否使用启用终端  
> StartupNotify=true
> StartupWMClass=确保程序启动后任务栏图标与 Icon 指定的图标一致，所以指定 WM_Class 值

示例：

```desktop
[Desktop Entry]
Name=Watt Toolkit
Comment=A cross-platform Steam toolbox.
Comment[zh_CN]=一个开源跨平台的多功能Steam工具箱。
Type=Application
Exec=/opt/SteamPP/Steam++.sh
Icon=/opt/SteamPP/Icons/Watt-Toolkit.png
# Terminal=false
StartupNotify=true
StartupWMClass=
Categories=Network;Utility
Keywords=Steam;Steam++;SteamTools;WattToolkit
```

3. 刷新
```shell
# 刷用户目录
update-desktop-database .local/share/applications
# 刷根目录
sudo update-desktop-database /usr/share/applications
```
> [!tip] 
> 
> 如果 desktop 不生效，可以使用 `desktop-file-validate xxx.desktop` 来检测 desktop 文件是否存在语法错误。

>[!info] 
>
> `StartupWMClass` 这个属性，指定应用程序在容器管理中的容器类名，确保应用程序在启动后，任务栏或应用切换器中显示的图标与 `.desktop` 文件中的图标一致，即使用 `icon` 指定的桌面图标。
> 
> 获取方法：执行 `xprop WM_CLASS` ，后点击运行中的窗口，获取窗口的 `WM_CLASS` 属性
> 

---

### <span id="linux_soft_install_komodo">Komodo Edit 安装 </span>

4. 下载
```shell
wget https://downloads.activestate.com/Komodo/releases/12.0.1/Komodo-Edit-12.0.1-18441-linux-x86_64.tar.gz
```

5. 解压
```shell
tar -zxvf xxx.tar.gz
```

6. 安装

转到新解压的目录，开始安装：
```shell
sudo ./install.sh -I /opt/KomodoEdit
```

7. 添加 PATH 变量 

`.bashrc` 或 `.bash_profile` 文件中添加
```shell
export PATH=$PATH:/opt/KomodoEdit/bin
```
source `.bashrc` 或 `.bash_profile` 

8. 如果需要在终端中调，可以添加软链：
```shell
sudo ln -s /opt/KomodoEdit/bin/komodo /usr/local/bin/komodo
```

---

## <span id="linux_grub">Grub</span>

grub 模板：`/etc/default/grub`

每当修改 `/etc/default/grub` 或者 `/etc/grub.d/` 中的文件之后，都需要再次生成主配置文件。

制成配置文件：`grub-mkconfig -o /boot/grub/grub.cfg`

重启电脑看效果。

## 常用选项

* `GRUB_TIMEOUT=10`：grub 等待时间，单位为秒
* `GRUB_GFXMODE`：分辨率，默认是 **auto**，可以配多个，如 `GRUB_GFXMODE=1920x1080,1280x720,1024x768,auto`
* `GRUB_DISABLE_OS_PROBER`：禁止使用 OS_PROBER（系统探针），默认值是 `false`，即不禁止**OS_PROBER**。**OS_PROBER**这东西是能探测操作系统的，操作系统变化，能及时的更新 Grub 的引导，所以最好不要修改，保持默认。

---

## 图形界面相关

### 显示服务器

#### 查询当前使用显示服务器

如果查询当前系统使用的是的 [Xorg](#Xorg) 还是 [Wayland](#Wayland)：

```shell
echo $XDG_SESSION_TYPE
```

示例：

```shell
# silascript @ (base) in ~ [21:28:21] 
$ echo $XDG_SESSION_TYPE 
wayland
```

#### Xorg

#### Wayland

#### 常见问题

##### wayland 不加载 xprofile

wayland 模式下，系统是不会加载 `.profile` 或 `.xprofile` 文件的，即便是手动 `source`，也属于临时性的，关闭 Terminal 后，加载效果恢复原状。

因为这个特性，让原有习惯的配置环境变量的方式处于一种「作废」的状态。

###### 解决方案 1

9. 将环境变量集中到自定义的配置文件中，比如 `.local_profile`，配置文件的文件名可以自己取，当然为了延续习惯，还是叫 `xxprofile` 好点。
10. 如果使用 [Zsh_note](zsh_note.md)，可以在 `.zshrc` 文件中 `source` 自定义的配置文件。如果只使用 [Bash](Shell/Shell_Note.md#Bash)，就在 `.bashrc` 或 `.bash_profile` 中 `source` 自定义配置文件。
> [!note] 
> 
> 如果使用 [Zsh](zsh_note.md)，只要在 `.zshrc` 中 `source` 下，连 [Bash](Shell/Shell_Note.md#Bash) 也生效。
11. 如果当前是 [Xorg](#Xorg)，类似的，在 `.xprofile` 或 `.profile` 中 `source` 自定义的配置文件。

> [!note] 
> 
> 如果为了兼容性，可以在 `.zshrc` 或 `.xprofile` 文件 `source` 配置文件时，加个 `if` 语句判断下：
> 
> `if [[ $XDG_SESSION_TYPE == "x11" ]]` 或 `if [[ $XDG_SESSION_TYPE == "wayland" ]] && [[ -f $HOME/.local_profile ]]` 
> 
> 这个 `.xprofile` 就能在 [Wayland](#Wayland) 模式下得以保留 -- 保留这货，可能对于某些软件是有必要的，如 [SublimeText](../Editors/Editors_Note.md#editors_sublime)。
> 

示例：

* [我自己的.zshrc配置](https://github.com/silascript/LinuxConfigs/blob/master/manjaro/x11/.zshrc)
* [我自己的.xprofile配置](https://github.com/silascript/LinuxConfigs/blob/master/manjaro/x11/.xprofile)

>[!tip] 
>
>
>不要使用 `.zshenv` 去配置或 `source` 环境变量，这货，每次 `source` 或打开一次 Terminal，都会「叠加」一次环境变量，这样就会出现重复，很可怕。个人看来 `.zshenv` 这货基本就已经是废了，[Zsh](zsh_note.md) 怪不得各种资料都不太提这货，一说配置都只说在 `.zshrc` 里配，这大概是其中一个原因罢。

###### 解决方案 2

[解决方案 1](#解决方案%201) 只能解决 Wayland 在 Terminal 这种 Shell 下环境变量的问题，对于一些图形界面软件，则这些环境变量则依然未生效，如 [VSCode](../Editors/VSCode_Note.md)，虽然使用 [解决方案 1](#解决方案%201)，在 Terminal 下 `PATH` 是配置正确的，但在 VSCode 下却依然找不到相应的配置，这证明了 [解决方案 1](#解决方案%201) 存在明显局限性。

要想解决，就得使用类似于在 [Xorg](#Xorg) 下 `.xprofile` 文件相似能力的方式。

在 `~/.config/environment.d/` 下创建 `.conf` 文件，这个文件就是能达到类似于 `.xprofile`，无论是 Shell 还是其他软件，具有「全局性」的环境变量配置方案。

`.conf` 文件格式是「键=值」对，跟 `rc` 或 `profile` 类似，但比它们更「死板」--`rc` 或 `profile` 实则是 shell 脚本，所以使用起来很灵活，能使用 shell 语法写些逻辑代码，但 `.conf` 文件去真如其名，就只有个「配置文件」，所以使用上得先作下「心态调整」，不要期望它像 `rc` 或 `profile` 一样爽。

`.conf` 文件语法：

* `.conf` 文件中变量引用使用 `${}`，如 `${HOME}`

相关命令：

* `systemctl --user show-environment`：显示当前环境变量配置情况

> [!info] 相关资料
> 
> * [systemd/用户 - Arch Linux 中文维基](https://wiki.archlinuxcn.org/wiki/Systemd/%E7%94%A8%E6%88%B7#%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
> * [systemd/User - ArchWiki](https://wiki.archlinux.org/title/Systemd/User#Environment_variables)
> * [Environment variables - ArchWiki](https://wiki.archlinux.org/title/Environment_variables)
> * [environment.d(5) — Arch manual pages](https://man.archlinux.org/man/environment.d.5)

###### 解决方案 3

此方案是迄今为此，最佳解决方案。

使用 `.zprofile` 替代 `xprofile` 作为「全局」配置文件，这即**不用使用** `~/.config/environment.d/` 目录下的 `.conf` 配置文件，也**不用**使用 `.xprofile` 来「兼容」两种模式，甚至**不用**在配置文件中使用 `if [[ $XDG_SESSION_TYPE == "wayland" ]]` 这种判断语句，可以说这是最简洁高效的方案。

> [!info] 
> 
> 使用 `.zprofile`，虽然在 `x11` 和 `Wayland` 两种模式下都能使用环境变量生效，而且不止是在 [Shell](Shell/Shell_Note.md) 中生效，图形界面的软件同样也能生效。
> 
> 但这个方案不是完全完美方案，因为如果配置文件中 `eval` 配置相关环境变量语法时，在 `x11` 模式下，环境变量配置是无效。
> 
> 在当前各 [Linux_Note](Linux_Note.md) 各大发行版都逐步使用 [Wayland](#Wayland) 的大趋势，[Xorg](#Xorg) 模式下存在一点小「bug」，应该也是可以接受的，毕竟 [Xorg](#Xorg) 已经开始处于「被淘汰」的状态了，所以使用 `.zprofile` 来代替 `.xprofile` 已经是「最优解」。
> 
>> [!tip] 
>> 
>> 但注意，[conda](../Python/Python_Note.md#python_conda) 那个 `conda init` 生成的配置代码，不能放在 `.zprofile` 中，虽然 conda 有效，但 `conda activate` 切换环境就失效了。所以为了 conda 的正常使用，还是将那段代码移到 `.zshrc` 文件中。
> 

##### gnome-tweak 启动问题

[conda](../Python/Python_Note.md#python_conda) 会引起 gnome-tweak 启动出错，可能会报 [`ValueError Namespace Gtk not available`](#`ValueError%20Namespace%20Gtk%20not%20available`) 这种错误。主要是 [conda](../Python/Python_Note.md#python_conda) 接管了 Python 环境造成的。

###### 解决方案

将 gnome-tweaks 的启动文件中的第一行，即 python 路径修改。

`#!/usr/bin/python3` 或 `#!/usr/bin/env python3`，自行使用 `whereis python` 来查询自己的 python 的路径，把路径改下就能启动了！

> [!tip] gnome-tweaks 启动文件
> 
> 使用 `whereis gnome-tweaks` 查看。一般是 `/usr/bin/gnome-tweaks` 这文件。

很多时候 gnome-tweaks 使用的是 `#!/usr/bin/env python3` 这个，而使用的 [conda](../Python/Python_Note.md#conda)，这个 python 就是 conda 下的 python，一般来说 linux 会自带默认的 python，所以修改为系统默认的 python3，即 `/usr/bin/python3`，就可以启动 gnome-tweak 了。

###### `ValueError: Namespace Gtk not available`

运行：`conda install -c conda-forge gtk3`。不过很多时候这个错误报出只是表象，更多的是 gnome-tweak 使用了 conda 的 python，而 conda 中是没有 gtk 的，所以还是使用 [gnome-tweaks 启动出错解决方案](#解决方案) 中的修改 gnome-tweaks 的 python 路径来解决。

相关资料：

* [警惕conda造成的环境问题! | 三个技术小站](https://qsctech-sange.github.io/anaconda-problems.html)
* [ValueError: Namespace Gtk not available 的解决方案](https://blog.csdn.net/qq_53937391/article/details/128125174)

##### theme

主题以目录来区隔，可以放在根也可以放在用户目录：

* `/usr/share/themes/`
* `~/.local/share/themes/`

一般使用系统的包管理器安装的主题，都装在根上，即 `/usr/share/themes/` 这个目录下。

而为了方便还是建议建在用户目录，即 `~/.local/share/themes`

> [!tip] 
>
> [gnome-look](https://www.gnome-look.org/)：这个网站拥有大量 gnome 美化相关的资源。

###### GTK 主题相关

GTK 主题比较突出的一个问题，就是它不随着 Gnome「深浅色模式」切换而切换，虽然使用 tweak 工具可以设，但设完，系统重启或切换深浅色械，它就立即失效。

为了解决这个问题，可以使用 Gnome-Shell 的一个扩展插件：[night-theme-switcher](https://extensions.gnome.org/extension/2236/night-theme-switcher/) 。

night-theme-switcher 这个插件其中有个设置，为「日间」和「夜间」，即深色浅色模式，指定相应的主题 -- 这是自动切换。如果不管日间还是夜间都想用深色主题，那都设置为深色主题就好了。这个小插件非常方便地解决了 GTK 主题不跟 Gnome 深浅色模式切换的「小 Bug」。

> [!info] 参考资料
>
> * [Gnome 美化](https://zh.opensuse.org/Gnome_%E7%BE%8E%E5%8C%96)
> * [auto-darkmode-on-gnome](https://envs.net/~grassblock/post/auto-darkmode-on-gnome/)

#### 打开文件夹问题

其实是默认应用问题，只要以下代码就能回复打开文件夹功能：

```shell
xdg-mime default org.gnome.Nautilus.desktop inode/directory
```

---

## 小工具

### cheat.sh

[cheat.sh/:firstpage](https://cheat.sh/) 是一个快速查询 Linux 命令的小工具。它不但能使用浏览器查询，还能在命令行中使用：

```shell
curl cheat.sh
```
> [!tip]
> 
> 也可以缩写成这样：`curl cht.sh`

如查询 [awk](#awk) 的使用，可以敲入这个命令：`curl cheat.sh/awk`，就会显示 awk 使用示例。

### navi

[navi](https://github.com/denisidoro/navi) 功能与 [cheat.sh](#cheat.sh) 类似，就是命令行查询，它是使用 [Rust](../Rust/Rust_Note.md) 写的。

### FileManager-Actions

FileManager-Actions 这个工具可以方便更改右键菜单。

### 取色器

#### Gcolor3

[Gcolor3](https://www.hjdskes.nl/projects/gcolor3/) [![Gcolor3 Repo](https://img.shields.io/github/stars/Hjdskes/gcolor3
)](https://github.com/Hjdskes/gcolor3) 是一个使用 GTK+3 制作的取色小工具。

![Gcolor3 screenshot](https://www.hjdskes.nl/img/projects/gcolor3/picker.png)

使用包管理器安装，以 [ArchLinux](ArchLinux_Note.md) 为例：

```shell
yay -S extra/gcolor3
```

### 彩色命令行

#### lsd

[lsd](https://github.com/lsd-rs/lsd) 是一个给 `ls` 命令美化的小工具。

![lsd screen](https://raw.githubusercontent.com/Peltoche/lsd/assets/screen_lsd.png)

#### exa

[exa](https://github.com/ogham/exa) 与 [lsd](#lsd) 类似功能的小工具 -- 同样也是使用 [Rust](../Rust/Rust_Note.md) 语言编写的。

![exa screen](https://github.com/ogham/exa/raw/master/screenshots.png)

exa 可以设置各种显示样式，像什么树型目录，git 标识等。

示例：
```bash
alias ll='exa -a --long --header --group --tree --level=2 --icons --time-style=long-iso'
```
> [!info]
> `-a`：显示所有文件包括隐藏文件
> 
> `--long`：完整路径
>
> `--icons`：显示目录及文件图标
> 
> `--header`：显示各列的名称
> 
> `--group`：显示用户组（默认只显示用户）
> 
> `--tree`：以树型结构显示目录结构
> 
> `--level=2`：显示子目录的层数
> 
> `--time-style=long-iso`：这是将时间样式设置成 `2023-05-31 01:37` 样式。
> 
> `--git`：如果当前目录是 [Git](../Git/Git_Note.md) 管理目录，将会多出一个显示 git 状态的列，否则这一列将不会显示。

更详细设置请参考：[exa 文档](https://the.exa.website/features)

> [!tip] 
> 
> exa 已经停止更新的，可以使用 [eza](#eza) 平替。

#### eza

[eza](https://github.com/eza-community/eza) 是 [exa](#exa) 的一个 fork 版本。因为 exa 已经停止更新的，所以可以使用 eza 进行替代。

> [!tip] 
> 
> 可以将做 `exa` 的软链接指向 `eza` 安装目录。这样之前使用了 [exa](#exa) 的配置将不用修改，无痛过度到 eza。

### 监控工具

#### lm_sensors

`lm_sensors` 是用来获取 CPU 温度的一个小工具。

执行 `sensors` 命令就能查看当前系统当前的温度列表，如：

```shell
$ sensors
acpitz-acpi-0
Adapter: ACPI interface
temp1:        +27.8°C  
temp2:        +29.8°C  

coretemp-isa-0000
Adapter: ISA adapter
Package id 0:  +42.0°C  (high = +82.0°C, crit = +102.0°C)
Core 0:        +42.0°C  (high = +82.0°C, crit = +102.0°C)
Core 1:        +42.0°C  (high = +82.0°C, crit = +102.0°C)

```

> [!info] 相关资料
> 
> * [获取Linux上的CPU温度](https://cn.linux-console.net/?p=10034)
> * [在 Linux 上监控 CPU 和 GPU 温度 - 知乎](https://zhuanlan.zhihu.com/p/67791270)

### 硬件检测

#### cpu-x

[CPU-X](https://github.com/TheTumultuousUnicornOfDarkness/CPU-X) 是 「Linux 版的 CPU-Z」，而且是开源的。

> [!info] 相关资料
>
> * [CPU-X 是 Linux 的 CPU-Z 的替代品 - 知乎](https://zhuanlan.zhihu.com/p/570458864)

#### hardinfo2

[hardinfo2](https://github.com/hardinfo2/hardinfo2) 是 hardinfo 的重构版。

> [!info] 相关资料
>
> * [使用 Hardinfo 以图形方式检查 Linux 上的硬件信息](https://cn.linux-console.net/?p=18500)
> * [Linux系统查看硬件信息神器，比设备管理器好用100倍！ - 良许Linux - 博客园](https://www.cnblogs.com/yychuyu/p/13373827.html)

### 相关链接

* [我的终端环境：高效 shell 命令（一）之目录文件 exa、zoxide 与 bat - POLOXUE's BLOG](https://www.poloxue.com/posts/2023-10-28-high-productivity-shell-commands-part1/)
* [modern-unix](https://github.com/ibraheemdev/modern-unix) ：这是一个 Unix/Linux 命令行小工具合集库。

---

## <span id="linux_notes">其他 Linux 笔记</span>

* [ArchLinux 笔记](ArchLinux_Note.md)
* [CentOS 笔记](CentOS_Note.md)
* [Fedora 笔记](Fedora_Note.md)
* [Ubuntu 笔记](Ubuntu_Note.md)
* [Debian 笔记](Debian_Note.md)
* [Shell 笔记](Shell/Shell_Note.md)
* [Linux 视频清单](./Linux_Videos.md)
* [Shell 视频清单](Shell/Shell_Videos.md)
* [Ranger 相关笔记](Ranger_Note.md)
* [AWK 笔记](AWK_Note.md)

---

## <span id="linux_resource_links">相关资源链接</span>

### 社区网站

* [Linux迷](https://www.linuxmi.com/)
* [linux-command](https://github.com/jaywcjlove/linux-command)
* [LinuxStory](https://linuxstory.org)

### 各种资料

* [Linux 资料清单](Linux_Material.md)

