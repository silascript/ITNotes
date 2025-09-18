---
aliases: []
tags:
  - flutter
  - dart
created: 2025-09-19 02:05:06
modified: 2025-09-19 02:57:04
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

## 安装与配置

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

[fvm](https://github.com/befovy/fvm)  是 Flutter 多版本管理工具，可以快速切换版本。

#### puro

[puro](https://github.com/pingbird/puro) 同样是一款 Flutter 多版本管理工具。

---

## 相关笔记

* [Dart 笔记](Dart_Note.md)

