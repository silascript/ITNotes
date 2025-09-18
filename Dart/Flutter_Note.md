---
aliases: []
tags:
  - flutter
  - dart
created: 2025-09-19 02:05:06
modified: 2025-09-19 04:57:18
---

# Flutter 笔记

---

## 简介

[Flutter](https://github.com/flutter/flutter) 是由 [Google](google.com) 开发的开源跨平台应用软件开发工具包。

Flutter 是使用 [Dart](Dart_Note.md) 语言编写。

> [!info] 
> 
> 相关网站
> 
> * [ Flutter 中文开发者网站 - Flutter](https://flutter.cn)
>  

---

## SDK

Flutter SDK 分三个「channel」：

* Stable channel：稳定版
* Beta channel：beta 版。Main channel 发布两周后进入 beta channel。
* Main channel：未经全面测试的版本。

一般安装 Stable channel 的版本就好了。

不同版本的 Flutter SDK 对应不同版本 [Dart](Dart_Note.md) SDK 的版本，自行在官方文档中 SDK 表格中查看对应关系：[归档列表 - Flutter](https://docs.flutter.cn/install/archive#stable-channel)

### 安装

要使用 Flutter 就得装相应的 SDK。

#### 安装方式

###### 手动安装

可以下载相应的 SDK 包，解压并配置环境变量方式来安装。

###### 包管理器安装

如果是 [Linux](../Linux/Linux_Note.md) 也可以通过相应的包管理器来安装。

###### 多版本管理工具安装

可以使用 [多版本管理](#多版本管理) 工具进行 Flutter 安装及版本管理。

> [!important] 
> 
> 无论是安装还是使用，因为「墙」的原因，国内都得配相关的 [镜像](#镜像)。

#### 镜像

Linux 下配置 Flutter 镜像：

```shell
export PUB_HOSTED_URL=https://pub.flutter-io.cn;
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

> [!info] 
> 
> 其他镜像配置示例
> 
> 上海交大镜像配置：
>  
> ```shell
> export PUB_HOSTED_URL=https://mirror.sjtu.edu.cn/dart-pub;
> export FLUTTER_STORAGE_BASE_URL=https://mirror.sjtu.edu.cn
> ```
> 
> 清华镜像配置：
> ```shell
> export PUB_HOSTED_URL=https://mirrors.tuna.tsinghua.edu.cn/dart-pub;
> export FLUTTER_STORAGE_BASE_URL=https://mirrors.tuna.tsinghua.edu.cn/flutter
> ```

 * [Flutter 可信的镜像站点](https://docs.flutter.cn/community/china/#known-trusted-community-run-mirror-sites)

### 多版本管理

#### fvm

[fvm](https://github.com/leoafarias/fvm) 是 Flutter 多版本管理工具，可以快速切换版本。

> [!info] 
> 
> 注意有多个同名的 fvm，选 star 最多的那个。中文那个已经四、五年都没更了！

FVM 命令：

```shell
api        JSON API for FVM data
config     Set global configuration settings for FVM
dart       Proxies Dart Commands
destroy    Destroy FVM cache by deleting FVM directory
doctor     Shows information about environment, and project configuration.
exec       Executes scripts with the configured Flutter SDK
flavor     Executes a Flutter command using a specified version defined by the project flavor
flutter    Proxies Flutter Commands
global     Sets Flutter SDK Version as a global
install    Installs Flutter SDK Version
list       Lists installed Flutter SDK Versions
releases   View all Flutter SDK releases available for install. 
remove     Removes Flutter SDK Version
spawn      Spawns a command on a Flutter version
use        Sets Flutter SDK Version you would like to use in a project
```

##### 常用命令

###### 列出可安装的版本

`fvm releases`

###### 安装

`fvm install 版本号`

示例：

```shell
$ fvm install 3.35.4
Creating local mirror...
 Counting objects:    [██████████████████████████████████████████████████] 100%
 Compressing objects: [█████████████████████████████████████████.........] 81%
✓ Clone complete
✓ Flutter SDK: SDK Version : 3.35.4 installed! 
```

###### 设置

设置全局版本

`fvm global 版本号`

示例：

```shell
$ fvm global 3.32.8
Flutter SDK: SDK Version : 3.32.8 is now global
```

###### 列出已经安装的版本

`fvm list`

示例：

```shell
$ fvm list         
Cache directory:  /home/silascript/fvm/versions
Directory Size: 305.86 MB

┌─────────┬─────────┬─────────────────┬──────────────┬──────────────┬────────┬───────┐
│ Version │ Channel │ Flutter Version │ Dart Version │ Release Date │ Global │ Local │
├─────────┼─────────┼─────────────────┼──────────────┼──────────────┼────────┼───────┤
│ 3.35.4  │         │ Need setup      │              │              │        │       │
├─────────┼─────────┼─────────────────┼──────────────┼──────────────┼────────┼───────┤
│ 3.32.8  │         │ Need setup      │              │              │ ●     │       │
└─────────┴─────────┴─────────────────┴──────────────┴──────────────┴────────┴───────┘
```

> [!info] 
> 
> 这比 [Ruby](../Ruby/Ruby_Note.md) 的 [Frum](../Ruby/Ruby_Note.md#Frum) 及 [NodeJS](../Node/NodeJS_Note.md) 的 [fnm](../Node/NodeJS_Note.md#fnm) 都强多了，还以表格显示。

##### 目录及配置

###### 配置

通过 `fvm config` 查看配置，默认配置文件是 `~/.config/fvm/.fvmrc`。

```shell
$ fvm config              
FVM Configuration:
Located at /home/silascript/.config/fvm/.fvmrc

No settings have been configured.
```

###### 安装目录配置

通过 fvm 安装的 Flutter SDK 默认是装在 `~/fvm/versions` 目录下。

通过 `fvm config --cache-path 目录路径` 来进行配置此目录。

###### 配置环境变量

跟 [Java](../Java/Java_Note.md) 的 [SDKMan](../Java/Java_Note.md#java_sdkman)、[Ruby](../Ruby/Ruby_Note.md) 的 [Frum](../Ruby/Ruby_Note.md#Frum) 及 [NodeJS](../Node/NodeJS_Note.md) 的 [fnm](../Node/NodeJS_Note.md#fnm) 这些多版本管理器一样，得把 fvm 的环境变量配上，这样才能通过 fvm「映射」并切换不同版本。

首先，fvm 安装目录，如默认 `~/fvm` 目录下，有一个 `default` 软连接，这是指向当前已经设置版本，即 `versions` 目录中的真正的版本。这跟 [SDKMan](../Java/Java_Note.md#java_sdkman) 中的 `.sdkman/candidates/xxx/current` 类似。所以配置 FVM 环境变量，就是把这个 `default` 链接配上即可。

```shell
$ ll fvm/                   
Permissions Size User       Group      Date Modified    Name
drwxr-xr-x     - silascript silascript 2025-09-19 04:25 .
drwx------     - silascript silascript 2025-09-19 04:23 ..
drwxr-xr-x     - silascript silascript 2025-09-19 04:20 cache.git
lrwxrwxrwx     - silascript silascript 2025-09-19 04:25 default -> /home/silascript/fvm/versions/3.32.8
drwxr-xr-x     - silascript silascript 2025-09-19 04:24 versions
```

#### puro

[puro](https://github.com/pingbird/puro) 同样是一款 Flutter 多版本管理工具。

---

## 相关笔记

* [Dart 笔记](Dart_Note.md)

