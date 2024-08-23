---
aliases: []
tags:
  - node
  - nodejs
created: 2023-08-19 23:06:10
modified: 2024-08-23 12:35:34
---

# NodeJS 笔记

---

## 目录
* [安装和配置](#node_insetings)
    * [安装](#node_install)
    * [配置](#node_settings)
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

参考资料：

* [正确设置 ELECTRON_MIRROR](https://newsn.net/say/electron-mirror.html)
* [Electron 环境配置 - 知乎](https://zhuanlan.zhihu.com/p/676814265)
* [npm安装Electron项目失败报错问题和解决方法](https://blog.csdn.net/weixin_46525113/article/details/132299107)

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

## 相关笔记

* [NodeJS 视频清单](NodeJS_Videos.md)
