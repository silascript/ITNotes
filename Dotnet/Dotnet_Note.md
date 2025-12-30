---
aliases: []
tags:
  - dotnet
  - csharp
created: 2024-07-18 17:32:19
modified: 2025-12-30 20:46:23
---

# .Net 笔记

---

## 历史

### 版本

|      目标      | 版本 | C# 语言版本的默认值 |
|:--------------:|:----:|:-------------------:|
|      .NET      | 10.x |        C# 14        |
|      .NET      | 9.x  |        C# 13        |
|      .NET      | 8.x  |        C# 12        |
|      .NET      | 7.x  |        C# 11        |
|      .NET      | 6.x  |        C# 10        |
|      .NET      | 5.x  |       C# 9.0        |
|   .NET Core    | 3.x  |       C# 8.0        |
|   .NET Core    | 2.x  |       C# 7.3        |
| .NET Standard  | 2.1  |       C# 8.0        |
| .NET Standard  | 2.0  |       C# 7.3        |
| .NET Standard  | 1.x  |       C# 7.3        |
| .NET Framework | 全部 |       C# 7.3        |

> [!info] 
> 
> [语言版本控制 - C# reference \| Microsoft Learn](https://learn.microsoft.com/zh-cn/dotnet/csharp/language-reference/language-versioning)

.NET [SDK](#SDK) 的版本，偶数版本为 LTS 版本（Long Term Support），即「长期支持版」。

---

## DotNet Framework

Windows 平台独占的 .NET 框架。

## DotNet Core

.NET 从 Windows 独占到跨平台中的过渡形式。其实就是微软想将.NET 的代码进行「跨平台化」，首先进行「改造」的是最**核心**的那些代码，所以称为「Core」。最终形态就是**DotNet**。

.NET 5 是微软推出的下一代.NET，标志着.NET Core 和 [.NET Framework](#DotNet Framework) 的合并。从.NET 5 开始，微软不再区分「.NET Core」和「.NET Framework」，而是提供一个统一的平台，支持所有类型的.NET 应用程序开发，包括桌面应用程序、Web 应用程序、云服务和移动应用程序。.NET 5 及其后续版本（如.NET 6、.NET 7 等）继续沿用.NET Core 的开源和跨平台特性，同时引入了更多的性能改进和功能增强。

.NET 5 是.NET 完成了「跨平台化」的一个里程碑版本， [.NET Framework](#DotNet Framework) 的「历史使命」已经完成，.NET 将进入跨平台时代。

---

## SDK

[.NET SDKs](https://dotnet.microsoft.com/en-us/download/visual-studio-sdks) 下载页面下载对应的 SDK 进行安装。

如果是 Linux，可以使用 Linux 自带的包管理器安装相应的版本。

Linux 是安装在 `/usr/share/dotnet` 目录下：

```shell
$ ll /usr/share/dotnet 
Permissions Size User Group Date Modified    Name
drwxr-xr-x     - root root  2025-09-10 11:50 .
drwxr-xr-x     - root root  2025-09-06 02:53 ..
.rwxr-xr-x   70k root root  2025-09-10 11:48 dotnet
drwxr-xr-x     - root root  2025-01-18 23:41 host
drwxr-xr-x     - root root  2025-02-05 01:06 packs
drwxr-xr-x     - root root  2025-09-17 01:11 sdk
drwxr-xr-x     - root root  2025-03-18 02:27 sdk-manifests
drwxr-xr-x     - root root  2025-02-05 01:00 shared
drwxr-xr-x     - root root  2025-09-17 01:11 templates
```

不同版本的 SDK 都是在 `/usr/share/dotnet/sdk` 目录下的子目录：

```shell
$ ll /usr/share/dotnet/sdk 
Permissions Size User Group Date Modified    Name
drwxr-xr-x     - root root  2025-09-17 01:11 .
drwxr-xr-x     - root root  2025-09-10 11:50 ..
drwxr-xr-x     - root root  2025-09-17 01:11 8.0.119
drwxr-xr-x     - root root  2025-08-30 00:29 9.0.109

```

### CLI

dotnet cli 是 dotnet 的命令行工上，用于执行各种开发任务，如构建、运行、发布等。

只要安装了 .NET SDK 后就带上了 dotnet CLI。

> [!tip] 
> 
> dotnet CLI 都以 `dotnet` 命令为始，后面加子命令及选项或参数。

```shell
add               将包或引用添加到 .NET 项目。
build             生成 .NET 项目。
build-server      与由生成版本启动的服务器进行交互。
clean             清理 .NET 项目的生成输出。
format            将样式首选项应用到项目或解决方案。
help              在浏览器中打开指定命令的引用页。
list              列出 .NET 项目的包或引用。
msbuild           运行 Microsoft 生成引擎(MSBuild)命令。
new               创建新的 .NET 项目或文件。
nuget             提供其他 NuGet 命令。
pack              创建 NuGet 包。
publish           发布 .NET 项目进行部署。
remove            从 .NET 项目中删除包或引用。
restore           还原 .NET 项目中指定的依赖项。
run               生成并运行 .NET 项目输出。
sdk               管理 .NET SDK 安装。
sln               修改 Visual Studio 解决方案文件。
store             在运行时包存储中存储指定的程序集。
test              使用 .NET 项目中指定的测试运行程序运行单元测试。
tool              安装或管理扩展 .NET 体验的工具。
vstest            运行 Microsoft 测试引擎(VSTest)命令。
workload          管理可选工作负荷。
```

### 常用命令

#### dotnet new

`dotnet new` 命令基于 [模板](#模板) 创建 .NET 项目或其他项目。

### 模板

#### 列出已安装的模板

```shell
dotnet new list
```

#### 安装模板

以 [Avalonia](Avalonia_Note.md) 为例：

```shell
dotnet new install Avalonia.Templates
```

### 全局工具

使用 `dotnet tool` 命令， 如果是全局操作就添加上 `--global` 参数。

#### 安装

语法：`dotnet tool install  工具名`

例：`dotnet tool install --global dotnet-suggest`

如果要指定版本号：`dotnet tool install --global dotnet-suggest --version 1.1.635903`

#### 卸载

语法：`dotnet tool uninstall 工具名`

示例：

```shell
$ dotnet tool uninstall --global dotnet-suggest
已成功卸载工具“dotnet-suggest”(版本“1.1.635903”).
```

#### 常用工具

##### dotnet-suggest

[dotnet-suggest](https://www.nuget.org/packages/dotnet-suggest) 是一个.NET 的全局提示工具。

### 相关文档

* [C# 文档 \| Microsoft Learn](https://learn.microsoft.com/zh-cn/dotnet/csharp/tour-of-csharp/)
* [dotnet 命令 - .NET CLI \| Microsoft Learn](https://learn.microsoft.com/zh-cn/dotnet/core/tools/dotnet)
* [全局工具](https://learn.microsoft.com/zh-cn/dotnet/core/tools/global-tools)

---

## 框架

### WPF

* [WPF 笔记](WPF_Note.md)

### Avalonia

* [Avalonia 笔记](Avalonia_Note.md)

---

## 相关笔记

* [.Net 资料清单](Dotnet_Material.md)
* [.Net 视频清单](Dotnet_Videos.md)
* [SQLServer 视频清单](../DataBase/SQLServer/SQLServer_Videos.md)

