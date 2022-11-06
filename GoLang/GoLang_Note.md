# Go 语言笔记

---
## 目录

* [安装和设置](#golang_setup)
	* [设置代理](#golang_setup_proxy)
	* [安装模块](#golang_setup_modules)

---

## <span id="golang_setup">安装和设置</span>

下载、解压，最后配 `PATH` 环境变量。
> 如果升级新版本，最后不要用新版直接覆盖旧版，这样容易在安装模块时出问题。最后先把旧版目录删除，新版的目录命令成一样的，复制到一样的路径下。这样升级，不用重新改`PATH` 环境变量，也能使用 go。

环境变量示例：
```shell
export PATH=$PATH:/opt/golang/bin
export PATH=$PATH:$(go env GOPATH)/bin
```


go 的环境配置是存放在 `~/.config/go/` 目录下的 `env` 文件，可以使用任何一个文本编辑器编辑。

---

### <span id="golang_setup_proxy">设置代理</span>

[goproxy.io](https://goproxy.io) 国内代理网站。
> 或者： [另一个域名](https://proxy.golang.com.cn)

在`env` 文件中配置以下代码：
```
GO111MODULE=on
GOPROXY=https://proxy.golang.com.cn,direct
```

也可以使用命令进行配置：
```shell
got env -w G111MODULE=on
go env -w GOPROXY=https://goproxy.cn,direct
```

甚至能配多个：
```shell
go env -w GOPROXY=https://goproxy.cn,https://proxy.golang.com.cn,https://goproxy.io,direct
```

配置完了，可以使用 `go env` 命令查看环境变量，查询代理是否配置上了。

国内 Go 代理：
* [Goproxy.cn](https://goproxy.cn/) [![goproxy.cn Repo](https://img.shields.io/github/stars/goproxy/goproxy.cn?style=social)](https://github.com/goproxy/goproxy.cn)
> Goproxy.cn 是布置在 [七牛云](https://www.qiniu.com/) 上，所以稳定性应该可以保证。
* [GoProxy.io](https://goproxy.io/) [![goproxy.io Repo](https://img.shields.io/github/stars/goproxyio/goproxy?style=social)](https://github.com/goproxyio/goproxy)
> GoProxy.io 应该是有两个域名：  
> `https:/goproxy.io`  
> `https://proxy.golang.com.cn`

---

### <span id="golang_setup_modules">安装模块</span>

示例：
```shell
go install golang.org/x/tools/gopls@latest
```

---

