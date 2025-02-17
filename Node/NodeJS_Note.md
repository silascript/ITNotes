---
aliases: []
tags:
  - node
  - nodejs
  - nvm
  - fnm
  - npm
created: 2023-08-19 23:06:10
modified: 2025-02-17 21:12:28
---

# NodeJS 笔记

---

## 目录

* [安装和配置](#node_insetings)
    * [安装](#node_install)
    * [配置](#node_settings)
* [版本管理](#版本管理)
	* [nvm](#nvm)
	* [fnm](#fnm)
	* [nodenv](#nodenv)
 
---

## <span id="node_insetings">安装和配置</span>

### <span id="node_install">安装</span>

* [NodeJS 官网](https://nodejs.org/)
	* [NodeJS 官网中文](https://nodejs.org/zh-cn)
* [NodeJS 中文网](http://nodejs.cn/)

Windows 下安装，可以下安装包，也可以下压缩包，解压后，手动配置环境变量。

> [!tip] 
> 
> 推荐下载「LTS」，长期维护版，更稳定。

### <span id="node_settings">配置</span>

`config` 命令，顾名思义就是用来配置 node。

`npm config list`：列出当前配置项，用来查看是否配置成功了。

#### 配置全局目录及缓存目录

全局目录：`npm config set prefix "~/nodejs/node_global/"`  ^node_global ^21bf6e

缓存目录：`npm config set cache "~/nodejs/node_cache/"`

> [!tip] 缓存相关的命令
> 
> 清理：`npm cache clean --force`

这两项还是得配，特别是 [全局目录](#^21bf6e) 必须得配，不然当你更新时它会跑到 node 的安装目录下的 `node_modules` 更新，如果你的 node 装在根目录，那有可能报 [npm 下载权限不足](#npm%20下载权限不足) 的错误。

当使用 `npm config set` 命令设置一个配置项后，会在当前用户目录下生成一个 `.npmrc ` 配置文件。

#### 设置国内镜像

`npm config set registry https://registry.npmmirror.com`

> [!tip] 
> 
> ~~`http://registry.npm.taobao.org`~~ 这个镜像域名已经过期。
> 
> * [npm淘宝镜像最新官方指引（2023.08.31） - 知乎](https://zhuanlan.zhihu.com/p/653480874)

#### eclectron 配置

在 `.npmrc` 文件中添加环境变量：

~~`export ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/`~~

`export ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/`

> [!info] 
> 
> 这里同样得注意淘宝的镜像过时问题。

参考资料：[NodeJS资料： Electron](NodeJS_Material.md#Electron)

`.npmrc` 配置示例：

```npmrc
prefix=~/nodejs/node_global/
cache=~/nodejs/node_cache/
registry=https://registry.npmmirror.com
ELECTRON_MIRROR=https://npmmirror.com/mirrors/electron/
```

#### 问题

##### 证书

2014 年后，npm 不再支持自签名证书，所以有可能会报相关的错误。

如：`Error: request to https://registry.npm.taobao.org/bash-language-server failed, reason: certificate has expired`。

如果出现这个可以使用取消严格 ssl 检查的设置：`npm config set strict-ssl false`

##### npm 下载权限不足

这个错误出现，往往是忘记设 [全局目录](#^21bf6e)，而 npm 更新会跑 node 的安装目录中的 `node_modules` 子目录寻找要更新的模块，而如果 node 的安装目录正好是在根目录，那自然会出现访问权限问题，这样就会报出 下载权限不足的错误。

其实观察错误也能推测出来，如下报错信息，明显可见，npm 是跑到的 `/` 根目录更新了：

```shell
npm ERR!   path: '/opt/NodeJS/node-v20/lib/node_modules/corepack',
npm ERR!   dest: '/opt/NodeJS/node-v20/lib/node_modules/.corepack-o8GX3Rw6'
```

> [!info] 相关资料
> 
> * [npm下载权限不足问题解决 - nobody阿欣 - 博客园](https://www.cnblogs.com/lixin-nobody/p/14051905.html)

---

## 版本管理

NodeJS 下有多款版本管理工具：

* [nvm](#nvm)
* [fnm](#fnm)
* [nodenv](#nodenv)

### nvm

[nvm](https://github.com/nvm-sh/nvm) 是在 [Linux](../Linux/Linux_Note.md) 下使用 [Shell](../Linux/Shell/Shell_Note.md) 写一款 NodeJS 版本管理工具。

> [!tip] 
> 
> Windows 版：[nvm-windows](https://github.com/coreybutler/nvm-windows)。

### fnm

[fnm](https://github.com/Schniz/fnm) 这是使用 [Rust](../Rust/Rust_Note.md) 写的 NodeJS 版本管理工具。跟 [Ruby](../Ruby/Ruby_Note.md) 那个 [Frum](../Ruby/Ruby_Note.md#Frum) 类似的东西。

fnm 有两特点：
1. 特点就是快
2. 跨平台。不像 [nvm](#nvm) 是 [Linux](../Linux/Linux_Note.md) 的，后来为了适配 Windows 又另外弄出一个 [nvm-windows](https://github.com/coreybutler/nvm-windows)。

![fnm screenshot](https://github.com/Schniz/fnm/raw/master/docs/fnm.svg)

#### 安装

[Linux](../Linux/Linux_Note.md) 下可以使用各自的包管理器安装。以 [ArchLinux](../Linux/ArchLinux_Note.md) 为例：

```shell
yay -S fnm
```

Windows 下推荐使用 [Scoop](../Scoop/Scoop_Note.md) 来安装：

```powershell
scoop install fnm
```

#### 常用参数及选项

```shell
list-remote  List all remote Node.js versions [aliases: ls-remote]
list         List all locally installed Node.js versions [aliases: ls]
install      Install a new Node.js version [aliases: i]
use          Change Node.js version
env          Print and set up required environment variables for fnm
unalias      Remove an alias definition
default      Set a version as the default version
current      Print the current Node.js version
uninstall    Uninstall a Node.js version [aliases: uni]

```

### nodenv

[GitHub - nodenv/nodenv: Manage multiple NodeJS versions.](https://github.com/nodenv/nodenv) 这个同样是使用 [Shell](../Linuxl/Shell_Note.md) 写的。

> [!info] 
> 
> 看名字，就知道跟 [Ruby](../Ruby/Ruby_Note.md) 那个 [rbenv](../Ruby/Ruby_Note.md#rbenv) 有所渊源。它确实是从 rbenv 分叉出来的 Node.js 的版本管理工具。

---

## 相关笔记

* [NodeJS资料](NodeJS_Material.md)
* [NodeJS 视频清单](NodeJS_Videos.md)
