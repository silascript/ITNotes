---
aliases: []
tags:
  - toml
created: 2025-09-22 13:21:57
modified: 2025-09-24 02:27:30
---

# TOML 笔记

---

## 工具

### Taplo

[Taplo](https://taplo.tamasfe.dev) [![taplo repo](https://img.shields.io/github/stars/tamasfe/taplo
)](https://github.com/tamasfe/taplo) 这是 [TOML_Note](../TOML/TOML_Note.md) 的一个 TOML 的工具。

主要功能：[LSP](../Protocols/LSP_Note.md) 和测试。

#### 安装

安装 Taplo 有多种方式。

* 通过系统的包管理器安装，如 `yay -S taplo-cli`

* 通过 [Cargo](../Rust/Rust_Note.md#rust_cargo) 来安装

```shell
cargo install taplo-cli --locked
```

或者：

```shell
cargo install --features lsp --locked taplo-cli
```

* [npm](../Node/NodeJS_Note.md#npm) 安装

```shell
npm install -g @taplo/cli
```

#### 功能

##### LSP

```shell
taplo lsp stdio
```

```shell
taplo lsp tcp --address 0.0.0.0:9181
```

##### 检测

检查 TOML 文件，是否存在语法等错误等。

```shell
taplo check foo.toml
```

##### 格式化

```shell
taplo fmt foo.toml
```

##### 转换文件格式

能将 TOML 转成其他格式的文件，如 JSON：

```shell
taplo get -f foo.toml -o json
```

---

## 相关网站

* [TOML：Tom 的（语义）明显、（配置）最小化的语言](https://toml.io/cn/)

---

## 相关笔记

* [Rust 笔记](../Rust/Rust_Note.md)