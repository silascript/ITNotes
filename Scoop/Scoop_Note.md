---
aliases: []
tags:
  - windows
  - scoop
  - git
created: 2023-01-13 12:27:46
modified: 2024-06-02 20:24:04
---

# Scoop 笔记

---

## 目录

* [安装 Scoop](#scoop_install)
* [卸载 Scoop](#scoop_uninstall)
* [使用 Scoop](#scoop_use)
	* [Bucket 相关](#scoop_bucket)
* [常用软件介绍](#scoop_softs)
	* [基础软件](#scoop_softs_basic)
		* [git](#scoop_softs_basic_git)

---

## <span id="scoop_install">安装 Scoop</span>

### 指定安装目录

方式 1：

```powershell
.\install.ps1 -ScoopDir 'F:\scoop\locals' -ScoopGlobalDir 'F:\scoop\global' -NoProxy
```

这种方式，官方推荐，但这种方式，会报 `Cannot find path 'C:\Users\xxx\scoop\buckets' because it does not exist.` 错误。因为是指定了目录，但装完 scoop 后，scoop 找 bucket 还是去 C 盘默认目录去找，这明显是找不到的，所以才会报错。

方式 2：

```powershell
$env:SCOOP='F:\scoop\locals'
$env:SCOOP_GLOBAL='F:\scoop\global'
[Environment]::SetEnvironmentVariable('SCOOP_GLOBAL', $env:SCOOP_GLOBAL, 'User')
iwr get.scoop.sh | iex
```

> [!info] 
> 
> 三句，为防止出问题，建议一句句执行。第一、二句是指定安装目录变量；第三句设置环境变量；第四句是下载和安装。

最好分开设置：

设置 scoop 目录：

```powershell
$env:SCOOP='F:\scoop\locals'
[Environment]::SetEnvironmentVariable('SCOOP', $env:SCOOP, 'User')
iwr -useb get.scoop.sh | iex
```

其实执行完这里，scoop 已经能用了。因为前两句代码只是将「SCOOP」这个变量加入到用户变量中，第三句才是安装 scoop。

设置全局目录：

```powershell
$env:SCOOP_GLOBAL='F:\scoop\global'
[environment]::setEnvironmentVariable('SCOOP_GLOBAL',$env:SCOOP_GLOBAL,'Machine')
```

其实全局这里设置只是将「SCOOP_GLOBAL」这个环境变量加入到「系统变量」中。

执行完以上操作，最好到「属性」-「高级设置」-「环境变量」里查看：
1. 「用户变量」里是否存在 **SCOOP** 变量
2. 「系统变量」中是否存在 **SCOOP_GLOBAL** 变量。

具体安装可参考：[ScoopInstaller/Install](https://github.com/ScoopInstaller/Install)

---

## <span id="scoop_uninstall">卸载 Scoop</span>
```powershell
scoop uninstall scoop
```

---

## <span id="scoop_configs">相关设置</span>

开启或关闭使用 `aria2`：
```powershell
scoop config aria2-enabled false
```

关闭 aria2 警告信息：
```powershell
scoop config aria2-warining-enable false
```

---

## <span id="scoop_use">使用 Scoop</span>

使用 scoop，可以先使用 `scoop checkup` 来检测下 scoop 存在什么问题。

---

### <span id="scoop_bucket">bucket 相关</span>

查询已知的 bucket
```powershell
scoop bucket known
```

* [main](https://github.com/ScoopInstaller/Main)： 默认的主库
* [extras](https://github.com/ScoopInstaller/Extras)： 官方扩展库
* [version](https://github.com/ScoopInstaller/Versions)： 是旧版本软件库
* [java](https://github.com/ScoopInstaller/Java)： java 相关的，如各种 jdk
* [dorado](https://github.com/chawyehsu/dorado)： 国内常用软件

添加或移除 bucket
```
scoop bucket add bucket名
scoop bucket rm bucket名
```

如果要添加第三方 bucket，就得加上 bucket 的 url。
```powershell
scoop bucket add bucket名 bucket的url
```

 > 有时 `scoop update` 后，main 库会丢失，所以每次 update 后先 `scoop bucket list` 查看下 bucket 情况。

---

### <span id="scoop_use_search">scoop search</span>

```shell
scoop search 软件名
```

---

### <span id="scoop_use_install">scoop install</span>

```shell
scoop install 软件名
```

全局安装：

```shell
scoop install 软件名 -g
```

安装特定版本的软件：

```shell
scoop install 软件@版本号
```

安装指定库的软件：

```shell
scoop install 库名/软件名
```

### <span id="scoop_use_uninstall">scoop uninstall</span>
```shell
scoop uninstall 软件名
```

卸载软件并移除所有配置文件：
```shell
scoop uninstall 软件 -p
```

卸载全局软件：
```shell
scoop uninstall 软件 -g
```

---

### <span id="scoop_use_update">scoop update</span>
更新 Scoop 及所有 bucket 但不更新软件
```shell
scoop update
```

更新某软件：
```shell
scoop update 软件
```

更新所有软件：
```shell
scoop update *
```

更新全局软件：
```shell
scoop update 软件 -g
```

---

### Scoop 查询 

查询已安装的软件：
```shell
scoop list
```

查看软件状态，看哪些软件可以升级：
```shell
scoop status
```

---

### <span id="scoop_use_clean">Scoop 清理</span>

删除已安装的软件的旧版本：
```shell
scoop cleanup *
```

删除旧版本及安装包：
```shell
scoop cleanup -k *
```

#### <span id="scoop_use_cache">Scoop 缓存清理</span>
```shell
scoop cache
```

清理 Scoop 缓存：
```shell
scoop cache rm *
```

---

## <span id="scoop_softs">常用软件介绍</span>

### <span id="scoop_softs_basic">基础软件</span>

基础软件是一些 curl、wget、git 和一些模拟 linux 命令等软件。

#### <span id="scoop_softs_basic_git">git</span>
git 是 scoop 基础软件，并且不单只装 git，如果执行 `scoop install git` ，装的是一个 套件。

这个 git 套件包括了 [git](https://git-scm.com/)、[7zip](https://www.7-zip.org/)。

---

#### <span id="scoop_softs_basic_linux">linux 模拟命令软件</span>

一些从 linux 移植而来的软件，以达到在 powershell 下能执行一些 linux 的常用命令。

##### <span id="scoop_softs_basic_linux_uutils">uutils-coreutils</span>

[uutils](https://github.com/uutils) 这是用 Rust 写的一套 Linux 命令集合项目。其中核心工具包是 [uutils-coreutils](https://github.com/uutils/coreutils)，像什么 ln、ls、cat、chmod、cp、touch 等常用命令都已实现了。这组工具包比 msys 那个类似的 [coreutils](http://www.mingw.org/wiki/msys) 实现更多的命令。

安装 uutils-coreutils：
```powershell
scoop install uutils-coreutils
```

##### <span id="scoop_softs_basic_linux_sudo">sudo</span>

上面那个 coreutils 没有 sudo 命令，所以想要使用这个命令得单独安装。
`scoop install sudo`

这个 `sudo` 命令，实际是另一个 linux 移植的集合 [lukesampson/psutils](https://github.com/lukesampson/psutils) 中的一个实现的命令：，只是这个集合比较小，只有 10 个命令。这个小集合是专门为了 Scoop 写的，应该是想让 Scoop 更好用而写的。

可以使用 psutils 中的一些命令来补充 [uutils-coreutils](https://github.com/uutils/coreutils) ，使得在 powershell 下能用更多的 linux 命令。

##### <span id="scoop_softs_basic_linux_scompletion">scoop-completion</span>

[scoop-completion](https://github.com/liuzijing97/scoop-completion) 是一个 scoop 命令补全命令，可以让使用 scoop 时更流畅快捷。

---

### <span id="scoop_softs_common">常用软件</span>

[crystaldiskinfo](https://crystalmark.info/en/) 是一款硬盘检测工具。这货还有一个美化版本：`crystaldiskinfo-shizuku` ：。

[lux](https://github.com/iawia002/lux) 是一个使用 go 语言写的视频下载器。这货原来叫 `annie`。这软件依赖 [ffmpeg](https://www.ffmpeg.org/) ，ffmpeg 是视频库，所以要么先安装 ffmpeg，要么安装 lux 时，scoop 自动先安装 ffmpeg：。

[Shotcut](https://www.shotcut.org/) [![shotcut repo](https://img.shields.io/github/stars/mltframework/shotcut?style=social)](https://github.com/mltframework/shotcut) 开源免费的视频剪辑软件。这货同样依赖 `ffmpeg`。

[Kdenlive](https://kdenlive.org/) 同样是一款跨平台的免费视频剪辑软件，同样依赖 [ffmpeg](https://www.ffmpeg.org/) 。

[Zeal](https://zealdocs.org/) 离线文档软件，码农「搬砖」的好助手！

[Renamer](https://www.den4b.com/products/renamer) 批量重命名软件，免费版已经够用的。

[snipaste](https://zh.snipaste.com/) 是一个功能强大的截图软件。暂时只支持 Windows 和 Mac 系统，看官网未来应该会出 Linux 版本。

---

### <span id="scoop_softs_programs">编程软件<span>

什么 [gcc](https://gcc.gnu.org/)、[llvm](https://llvm.org/)、[go](https://golang.google.cn/)、java、[rust](https://www.rust-lang.org/)、[python](https://www.python.org/)、[ruby](https://www.ruby-lang.org/en/) 就不用说了，直接 `install` 就好了，连环境变量都给你配好了！
> 注意下，`rust` ，应该是装 `rustup`，这是 rust 的一个安装工具，通过 rustup 来装 rust。关于 rust 详细信息，请参考 [Rust 笔记](../Rust/Rust_Note.md)。 

[Spring Tool Suite](https://spring.io/tools) 是整合了 Spring 开发插件包的 Eclipse，不想手动安装 Spring 插件包的就直接下这个用好了，人家帮你整合好了！ 先用 `scoop search sts` 和 `scoop info sts` ，看下软件具体信息，确认没错，再 `install`。
> eclipse 也是类似操作 

[dbeaver](https://dbeaver.io/) [![dbeaver repo](https://img.shields.io/github/stars/dbeaver/dbeaver?style=social)](https://github.com/dbeaver/dbeaver) 一个数据库管理软件。免费版本就够用了，完全可以替代 [Navicat](https://navicat.com.cn/)，不用再辛苦地去找什么「破解版」。这货功能很强，还能非常方便地对对应的数据库的驱动进行下载，不用再另加开浏览器找驱动。

#### <span id="scoop_softs_programs_mysql">scoop 安装和使用 MySQL</span>

##### 安装 MySQL
`scoop install mysql` 很简单

---

##### 启动 MySQL

安装完，scoop 会提示可以使用三种方式启动 MySQL：
```powershell
Run 'mysqld --standalone' or 'mysqld --console' to start the Database,or run following command as administrator to register MySQL as a service.
```

个人偏爱添加加服务这种方式启动 MySQL。

---

###### 添加 MySQL 服务

常见 MySQL 添加服务有四种形式：

1. `mysqld --install` 添加一个使用默认服务名称（默认应该是 MySQL）且 **自动启动** 的服务
2. `mysqld --install 服务名` 添加一个指定服务名称且 **自动启动** 的服务
3. `mysqld --install-manual` 添加一个使用默认服务名称，**手动启动** 的服务
4. `mysqld --install-manual 服务名` 添加一个指定服务名称，**手动启动** 的服务

> 在添加服务时，还指定其他 `mysqld` 命令的其他属性，如最常见的，就是指定默认配置文件的指定：`--defaults-file="xxx\scoop\locals\apps\mysql\current\my.ini"`

示例：
```powershell
mysqld --install-manual MySQL8 --defaults-file="F:\scoop\locals\apps\mysql\current\my.ini"
```

> 执行完全命令，出现 `Service successfully installed.` 信息，就证明添加 mysql 服务成功了！如不放心可以到「管理」-「服务」里查看。

> 如出现 `Install/Remove of the Service Denied!` 这个错误，就是权限不够，要么使用管理员 powershell 再执行一次上述命令，要么在当前非管理员 powershell 下，在命令前添加 `sudo` 来执行：`sudo mysqld --install-manual MySQL8 --defaults-file="F:\scoop\locals\apps\mysql\current\my.ini"` ，当前前提是，scoop 你得装了 `sudo`。

---

###### 启动 MySQL 服务

添加 MySQL 服务成功，如果是「手动启动」类型的服务，就需要启动服务。

windows 下有两个命令都可以启动 Windows 服务：
1. net

 `net start 服务名`
 `net stop 服务名`

2. sc

 `sc start 服务名`
 `sc stop 服务名`

###### net 命令与 sc 命令的差异

`net` 命令不是只针对于服务的，还可以用在网络、用户、登录等。
`sc` 命令是 `service` 命令的缩写，顾名思义，这是专门针对 Windows 服务的命令

两者使用有一点区别：
1. `net` 命令对于禁用的服务无效，而 `sc` 却能启动禁用状态的服务
2. `sc` 命令不但能开启或停止服务，而且可以新建、配置服务。`mysqld` 底层应该是调用的 `sc` 命令来实现 MySQL 服务的创建。
3. 对于重启服务而言，`net` 和 `sc` 存在一定的细节差异。
 一般重启服务，无非就是先「停止」服务，再「启动」服务。差异就在这两个操作上。
因为 `net` 属于更老的方式，所以它是一种「同步」管理方式，而 `sc` 是一种「异步」的管理方式。 
> * net is older – from the days of MS-DOS and OS/2, in fact.
> * sc only appeared with Windows NT
> 可以看出，`net` 是 MS-DOS 时代的产物，而 `sc` 是 WinNT 时代的东西。

`net stop mysql & net start mysql`，使用 `net` 重启 mysql 服务，命令会依次执行关闭和开启 mysql，在执行完 `net stop mysql` 这个命令后，**会等待** 确认服务关闭之后，才会执行 `net start mysql` 这个开启服务的指令。

`sc stop mysql & sc start mysql`，同样是重启，这句代码则会执行失败，因为 `sc stop mysql` 这个指令，只是发送了一个关闭 mysql 的信号，`sc` 并 **未等待** 和确认服务是否关闭，这时服务极大可能是未完成关闭的，所以执行 `sc start mysql` ，就会出错。

简单说，`net` 是线性的，而 `sc` 是非线性的。

---

##### 进入 MySQL 后修改密码

使用 scoop 安装的 MySQL 是没有初始密码的。 
直接执行：` mysql -P 3366 -u root -p`
> 如果你没有在 `my.ini` 配置文件中专门指定过 MySQL 的端口号，`-P` 这个参数可以省略

进到 MySQL 后，执行以下代码：
```powershell
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '新密码';
```

当然记得刷新下：`FLUSH PRIVILEGES;`

如果不放心的，可以查询下 `user` 表
```sql
select user,host,authentication_string from user;
```

---

##### <span id="scoop_softs_programs_mysql_links">MySQL 相关链接</span>

其他 MySQL 具体使用信息，请参考以下笔记：
* [MySQL 笔记](../DataBase/mysql/MySQL_Note.md)
* [MySQL 常用操作](../DataBase/mysql/MySQL常用操作.md)

---

### <span id="scoop_softs_beauti">美化相关</span>

#### <span id="scoop_softs_beauti_concfg">concfg</span>

[concfg](https://github.com/lukesampson/concfg) 本来不是用来美化的，而且用来导出 Windows console 的配置的。而 Windows console 配置无非就两种设置：字体和颜色。所以，这货就自然成了配色工具。而这货也非常贴心地内置了大量的配色方案。

![atelier-cave](https://github.com/lukesampson/concfg/raw/master/preset_examples/atelier-cave.png)

![bombyx](https://github.com/lukesampson/concfg/raw/master/preset_examples/bombyx.png)

![ocean](https://github.com/lukesampson/concfg/raw/master/preset_examples/ocean.png)

使用也简单： `concfg import 内置配色方案名称`
> [预览内置的配色方案](https://github.com/lukesampson/concfg/blob/master/preset_examples/README.md)

使用 `concfg presets` 命令，能列出所有内置配色方案的名称。

#### <span id="scoop_softs_beauti_pshazz">pshazz</span>

[pshazz](https://github.com/lukesampson/pshazz) 是个主题软件。

pshazz 内置了一些 theme，可以使用 `pshazz list` 命令列出内置 theme 名称。

pshazz 常用命令
* `pshazz use theme名称` ： 切换当前 theme。
* `pshazz get url`： 获取 theme

---

