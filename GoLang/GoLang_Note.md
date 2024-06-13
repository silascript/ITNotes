---
aliases: []
tags:
  - PL
  - golang
created: 2023-01-31 11:31:14
modified: 2024-06-12 19:51:30
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

下载、解压，最后配 `PATH` 环境变量。
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
> 

---

### <span id="golang_setup_proxy">设置代理</span>

#### <span id="golang_setup_proxy_goproxy">GOPROXY 设置</span>

[goproxy.io](https://goproxy.io) 国内代理网站。
> 或者： [另一个域名](https://proxy.golang.com.cn)

在 `env` 文件中配置以下代码：

```
GO111MODULE=on
GOPROXY=https://proxy.golang.com.cn,direct
```
> [!tip]
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
> Goproxy.cn 是布置在 [七牛云](https://www.qiniu.com/) 上，所以稳定性应该可以保证。
* [GoProxy.io](https://goproxy.io/) [![goproxy.io Repo](https://img.shields.io/github/stars/goproxyio/goproxy?style=social)](https://github.com/goproxyio/goproxy)
> [!tip] 关于 GoProxy.io
> 
> GoProxy.io 应该是有两个域名：  
> * `https:/goproxy.io` 
> * `https://proxy.golang.com.cn`
* [阿里 Go代理](https://mirrors.aliyun.com/goproxy/)

> [!info]
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

## Go 相关的资料

### 教程

* [Go 语言简明教程| 极客兔兔](https://geektutu.com/post/quick-golang.html)

### 文档 

* [golang 官网](https://go.dev/)
* [Go社区 Wiki](https://learnku.com/go/wikis)
* [Golang中文学习文档](https://golang.halfiisland.com/)
* [Go语言中文文档](https://www.topgoer.com/)

---

## 相关笔记

* [GoLang视频清单](GoLang_Videos.md)

