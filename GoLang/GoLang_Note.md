---
aliases: []
tags:
  - PL
  - golang
  - go
created: 2023-01-31 11:31:14
modified: 2025-08-10 04:28:22
---

# Go 语言笔记

---

## 目录

* [安装和设置](#golang_setup)
	* [设置代理](#golang_setup_proxy)
	* [安装模块](#golang_setup_modules)

---

## <span id="golang_instroduction">简介</span>

### Go 作者

* Rob Pike，Go 语言项目总负责人
> [!tip] 关于 Rob Pike 的谣言
> 
> 中文百科中说 Rob Pike 曾在 1980 年莫斯科奥运会上夺得射箭银牌，这完全是 **谣言**！
* Ken Thompson 「**Unix 之父**」
* Rober Griesemer 参与开发 [Java](../Java/Java_Note.md) HotSpot 虚拟机。

---

## <span id="golang_setup">安装和设置</span>

### 安装包下载

官网 [https://golang.org/](https://golang.org/) 偶尔会访问不了，可以使用国内镜像网站下载 Go 的安装文件：

* [https://golang.google.cn/dl/](https://golang.google.cn/dl/)
* [https://gomirrors.org/](https://gomirrors.org/)

---

### 配置

主要就是配 [GOPATH](#GOPATH)。

下载、解压，最后配 `PATH` 环境变量。
> [!info] 
> 
> 如果升级新版本，最后不要用新版直接覆盖旧版，这样容易在安装模块时出问题。最后先把旧版目录删除，新版的目录命令成一样的，复制到一样的路径下。这样升级，不用重新改 `PATH` 环境变量，也能使用 go。

环境变量示例：

```shell
export PATH=$PATH:/opt/golang/bin
export PATH=$PATH:$(go env GOPATH)/bin
```

> [!tip] GOPATH
> 
> 旧的版本，即 1.11 之前的版本，环境变量还得设置 `GOPATH`，新的版本就不用理了。
> 
> `GOPATH` 是在 `~/home/用户名/go` 这个目录。

go 的环境配置是存放在 `~/.config/go/` 目录下的 `env` 文件，可以使用任何一个文本编辑器编辑。

> [!example] 配置示例
> 
> ```profle
> export GOROOT=/opt/golang
> export PATH=$PATH:$GOROOT/bin
> export PATH=$PATH:$(go env GOPATH)/bin
> ```
> 

### GOPATH

GOPATH 分为三类：

1. Global GOPATH
2. Project GOPATH
3. Module GOPATH

#### Global GOPATH

全局 GOPATH，通常第三方库，便于所有项目引用。

#### Project GOPATH

#### Module GOPATH

---

### <span id="golang_setup_proxy">设置代理</span>

#### <span id="golang_setup_proxy_goproxy">GOPROXY 设置</span>

[goproxy.io](https://goproxy.io) 国内代理网站。
> [!info] 
> 
> 或者： [另一个域名](https://proxy.golang.com.cn)

在 `env` 文件中配置以下代码：

```
GO111MODULE=on
GOPROXY=https://proxy.golang.com.cn,direct
```
> [!tip]
> 
> `GO111MODULE` 是指 Go 1.11 后的版本使用 Module 方式

也可以使用命令进行配置：

```shell
go env -w G111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

甚至能配多个：

```shell
go env -w GOPROXY=https://goproxy.cn,https://proxy.golang.com.cn,https://goproxy.io,direct
```

配置完了，可以使用 `go env` 命令查看环境变量，查询代理是否配置上了。

国内 Go 代理：

* [Goproxy 中国](https://goproxy.cn/) [![goproxy.cn Repo](https://img.shields.io/github/stars/goproxy/goproxy.cn?style=social)](https://github.com/goproxy/goproxy.cn)
>[!tip] 
>
> Goproxy.cn 是布置在 [七牛云](https://www.qiniu.com/) 上，所以稳定性应该可以保证。
* [GoProxy.io](https://goproxy.io/) [![goproxy.io Repo](https://img.shields.io/github/stars/goproxyio/goproxy?style=social)](https://github.com/goproxyio/goproxy)
> [!tip] 关于 GoProxy.io
> 
> GoProxy.io 应该是有两个域名：  
> * `https:/goproxy.io` 
> * `https://proxy.golang.com.cn`
* [阿里 Go代理](https://mirrors.aliyun.com/goproxy/)

> [!info]
> 
> 下载的模块内容会缓存在 `$GOPATH/pkg/mod`

> [!tip] 清理模块缓存
>
> ```shell
> go clean --modcache
> ```

#### GOSUMDB 代理

[GOPROXY](#golang_setup_proxy_goproxy) 下载压缩包后，还会调用 `GOSUMDB` 来对文件进行哈希校验。这是 Go Module 的安全机制。

同样因为国内访问的问题，[sum.golang.org](https://sum.golang.org) 会连接超时，这样会导致下载流程无法完成。所以 `GOSUMDB` 也得进行代理。

[sum.golang.google.cn](https://sum.golang.google.cn) 这是国内的 GOSUMDB 代理网址。

通过以下命令就能完成代理设置：

```shell
go env -w GOSUMDB=sum.golang.google.cn
```

---

### <span id="golang_setup_modules">安装模块</span>

示例：

```shell
go install golang.org/x/tools/gopls@latest
```

---

## Go 命令

查看 go 版本：

```shell
go version
```

编译：

```shell
go build xxx.go
```

运行：

```shell
go run xxx.go
```

清除对象：

```shell
go clean
```

显示 go 相关环境属性：

```shell
go env
```

格式化：

```shell
fmt
```

下载包：

```shell
go get
```

安装包及其依赖：

```shell
go install 
```

列出包：
```shell
go list
```

### go mod 命令

#### 初始化

```shell
go mod init 项目名
```

#### go.mod

`init` 后，会在项目根目录下生成一个 `go.mod` 文件，其中会有以下内容：

```txt
module e01

go 1.21.3

```

> [!tip] go.mod 文件解析
> 
> `module e01`：module 的名称
> 
> `go 1.21.3`：当前项目使用的 go 的版本

---

## 多版本管理

### goenv

[goenv](https://github.com/go-nv/goenv)

#### 安装和配置

安装很简使用系统的包管理器装：以 [ArchLinux](../Linux/ArchLinux_Note.md) 为例：`yay -S goenv`

装完 goenv 后，最好在终端上跑下 `goenv` 命令，这样会在 `HOME` 目录下生成 `.goenv` 目录，这是当前用户 goenv 的根目录，下面配置 goenv 的环境变量时会配到这个目录。

##### 配置

在你的各种 `rc` 或 `profile` 文件中添加以下配置：

```shell
# 使用goenv来管理go
export GOENV_ROOT="$HOME/.goenv"
export PATH=$PATH:$GOENV_ROOT
eval "$(goenv init -)"
```

#### goenv 命令

```shell

local       Set or show the local application-specific Go version
global      Set or show the global Go version
shell       Set or show the shell-specific Go version
install     Install a Go version using go-build
uninstall   Uninstall a specific Go version
rehash      Rehash goenv shims (run this after installing executables)
version     Show the current Go version and its origin
versions    List all Go versions available to goenv
which       Display the full path to an executable
whence      List all Go versions that contain the given executable
```

* `local`：是查看当前正在使用的 go 版本
* `global`：全局 go 版本
* `install`：安装指定版本的 go
* `uninstall`：卸载指定版本的 go
* `version`：显示当前 go 的版本
* `versions`：列出已经安装的所有 go 的版本
* `which`：显示版本的安装的完整路径

> [!info] 
> 
> 跟 [rbenv](../Ruby/Ruby_Note.md#rbenv) 真的很像啊。
> 
> 更详细的命令请参考官方文档：[goenv/COMMANDS.md](https://github.com/go-nv/goenv/blob/master/COMMANDS.md)

##### 示例

`goenv install`：

```shell
# 安装指定版本的go
goenv install 1.24.6
```

`vesion`：

```shell
$ goenv versions       
  1.23.12
  1.24.6
```

`goenv global`：

```shell

$ goenv global 1.23.12

$ goenv global        
1.23.12

$ goenv versions      
* 1.23.12 (set by /home/silascript/.goenv/version)
  1.24.6

```

> [!tip] 
> 
> 如果之前没有指定全局版本，那 `goenv global` 命令敲完后，是不显示任何版本的，同样 `goenv versions` 中版本也是没有任何版本前带 `*` 号标示。

### GVM

[gvm](https://github.com/moovweb/gvm)

### version-manager

[version-manager](https://github.com/gvcgo/version-manager) 这个不单单是给 Go 语言多版本管理的工具，而是「多语言」的多版本管理工具。

> [!quote] 
> 
>  这是官方文档介绍：
>  
> VMR 是一款**简单**，**跨平台**，且经过**良好设计**的版本管理器，用于管理多种 SDK 以及其他工具。它完全是为了通用目的而创建的。
>
> 你可能已经听说过 fnm，gvm，nvm，pyenv，phpenv 等 SDK 版本管理工具。然而，它们很多都不能管理多种编程语言。像 asdf-vm 这样的管理器支持多种语言，但只适用于类 unix 系统，并且看起来非常复杂。因此，VMR 的出现主要就是为了解决这些问题。

### g

[g](https://github.com/voidint/g) 这是一个类似 [Ruby](../Ruby/Ruby_Note.md) 中 [Frum](../Ruby/Ruby_Note.md#Frum) 或 [NodeJS](../Node/NodeJS_Note.md) 中 [fnm](../Node/NodeJS_Note.md#fnm) 的 GoLang 多版本管理工具。

它跟 [Frum](../Ruby/Ruby_Note.md#Frum) 和 [fnm](../Node/NodeJS_Note.md#fnm) 一样，可以列出远程可以安装的版本，这省去了另开网页去找版本的步骤。

缺点：就是在 [zsh](../Linux/zsh_note.md) 中可能与 [Git](../Git/Git_Note.md) 的「别名」冲突，所以使用 zsh 的人，最好不要设置 git 的别名为 `g`。

---

## go 项目

### 项目结构

Go 规定的项目结构：
* `src`：源码目录
* `pkg`：编译后生成的文件
* `bin`：可执行文件

## 包

* 一个目录（文件夹）即称为一个「**包**」，在目录（包）中可以创建多个文件
* 在同一个包下的每个文件中**必须**指定包名称，且此包名相同

> [!info] Go 语言包三条件
> 
> 1. 同一目录下同级的所有 go 文件应属于一个包
>    
> 2. 包的名称可以跟目录名不同（这与 [Java_Note](../Java/Java_Note.md) 不同，与 c#语言的命名空间比较像）
>    
> 3. 一个 Go 语言程序**有且只有一个**`main` 函数，它是程序的入口函数，且必须属于 main 包。
>> [!tip] main 函数不在 main 包
>> 
>> 在运行时，就会报 `package command-line-arguments is not a main package ` 错误提示。

go 中**包**概念，可以简单看作两类：

一个是实际目录，可以叫它作「物理包」；  ^2330f2

另一个，是在 go 文件中，使用 `package` 定义的逻辑意义的包名，可简单称为「逻辑包」。   ^545dc0

这个区别非常重要，因为在使用 [import](#import)「导包」，还有调用函数时需要使用的包名调用，这两处的「包」的概念是不同的。

### import

包引入，使用 `import` 关键字。

引入某个目录：`import("module名/目录名")`

> [!tip] 关于 module 名称
> 
> 可以查看项目根目录下的 [go.mod](#go.mod) 文件。

```go

import (
	"e01/utils"
	"fmt"
)

```

> [!info] import 解析
> 
> `fmt`：go 内置包
> 
> `e01/utils`：导入的是 `e01` 这个 module 下 `utils` 目录

> [!import] 目录与 package
> 
> 在 `import` 时，是导入的是目录，即 [物理包](#^2330f2)；而在调用函数时，使用的是那个包，是 `package` 定义的 [逻辑包](#^545dc0) 名称。
> 

---

## 函数

### init 函数

`func init()` 函数是一个特殊的函数，在一个 go 文件中可以出现多次。`init` 函数是在 [import](#import) 包时，自动调用的函数，其调用顺序与 `import` 顺序相同。

---

## 教程和文档

* [Go 语言简明教程| 极客兔兔](https://geektutu.com/post/quick-golang.html)

### 文档

* [golang 官网](https://go.dev/)
* [Go社区 Wiki](https://learnku.com/go/wikis)
* [Golang中文学习文档](https://golang.halfiisland.com/)
* [Go语言中文文档](https://www.topgoer.com/)

---

## 相关笔记

* [Go语言视频清单](GoLang_Videos.md)
* [Go语言资料](GoLang_Material.md)

