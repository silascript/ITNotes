---
aliases: []
tags: []
created: 2023-08-19 23:06:10
modified: 2024-01-22 17:22:25
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
* [NodeJS 中文网](http://nodejs.cn/)

Windows 下安装，可以下安装包，也可以下压缩包，解压后，手动配置环境变量。

### <span id="node_settings">配置</span>

#### eclectron 配置

在 `.bashrc` 或 `.bash_profile` 或 `.profile` 文件中添加相应的环境变量：

`export ELECTRON_MIRROR=http://npm.taobao.org/mirrors/electron/`

参考资料：

* [正确设置 ELECTRON_MIRROR](https://newsn.net/say/electron-mirror.html)

#### 问题

##### 证书

2014 年后，npm 不再支持自签名证书，所以有可能会报相关的错误。

如：`Error: request to https://registry.npm.taobao.org/bash-language-server failed, reason: certificate has expired`。

如果出现这个可以使用取消严格 ssl 检查的设置：`npm config set strict-ssl false`

