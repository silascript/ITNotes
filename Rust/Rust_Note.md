# Rust 笔记

---
## 目录

* [安装与设置](#rust_insconf)
	* [Rust 是什么](#rust_about_rustup)
	* [Cargo 包管理器](#rust_about_cargo)

---

## <span id="rust_insconf">安装与设置</span>

打开 [Rust 官网](https://rust-lang.org)，官方推荐下载 Rustup 并安装 Rust。

---

### <span id="rust_install">安装</span>

执行 `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh` 命令。

会显示以下的安装选项：

```shell
Current installation options:

   default host triple: x86_64-unknown-linux-gnu
     default toolchain: stable
               profile: default
  modify PATH variable: yes

1) Proceed with installation (default)
2) Customize installation
3) Cancel installation

```

1：默认设置

2：自定义安装
  自定义安装中有一个「配置文件」选项，它是在安装Rust toolchain 时，选择下载组件组。它有三个选项，一个`最小`、`默认`和`完整`。

  「最小」只有基本的：rustc rust-std 和 cargo。

  「默认」在「最小」配置基础上加了 rust-docs、rustfmt和clippy。
  
  「完整」包括了元数据中包含的所有组件。

3：取消安装


除非必要，一路默认！

安装成功，会显示以下信息：
```shell
Rust is installed now. Great!

To get started you may need to restart your current shell.
This would reload your PATH environment variable to include
Cargo's bin directory ($HOME/.cargo/bin).

To configure your current shell, run:
source $HOME/.cargo/env

```

根据提示，`source` 下，让`~/.cargo/env` 这个配置文件生效。

`env` 这个文件是将cargo 配置到 `PATH` 环境变量中。

而如果安装选项选了 `modify PATH variable: yes`，`.cargo/env` 这个文件就会被配置到 `.bashrc`、`.bash_profile`、`.profile` 三个文件中：
  
```shell
. "$HOME/.cargo/env"
```

---

#### <span id="rust_install_windows">windows 下安装 Rust</span>

windows 下可以通过 [scoop](https://scoop.sh/) [![scoop repo](https://img.shields.io/github/stars/ScoopInstaller/scoop?style=social)](https://github.com/ScoopInstaller/scoop) 来安装 rust，很方便。
```powershell
scoop install rustup
```

使用 scoop 安装的 rust，`.rustup` 和 `.cargo` 两目录是被放在 `persist` 目录下，也就是说就算重装 windows 系统，之前下载装的 rust 的工具仍在。


---

### <span id="rust_rustup">Rustup</span>


#### <span id="rust_rustup_about">Rustup 是什么</span>

[![Rustup Repo]()](https://github.com/rust-lang/rustup.rs) 是一个 Rust 工具链安装器（the Rust toolchain installer），专门用于安装 Rust，也能对 Rust 进行管理：安装、升级、卸载等，还能切换版本。
> Rust 包括 stable、beta 和 nightly 三个版本。

`rustup` 命令：
```shell
rustup 1.24.3 (ce5817a94 2021-05-31)
The Rust toolchain installer

USAGE:
    rustup [FLAGS] [+toolchain] <SUBCOMMAND>

FLAGS:
    -v, --verbose    Enable verbose output
    -q, --quiet      Disable progress output
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <+toolchain>    release channel (e.g. +stable) or custom toolchain to set override

SUBCOMMANDS:
    show           Show the active and installed toolchains or profiles
    update         Update Rust toolchains and rustup
    check          Check for updates to Rust toolchains and rustup
    default        Set the default toolchain
    toolchain      Modify or query the installed toolchains
    target         Modify a toolchain's supported targets
    component      Modify a toolchain's installed components
    override       Modify directory toolchain overrides
    run            Run a command with an environment configured for a given toolchain
    which          Display which binary will be run for a given command
    doc            Open the documentation for the current toolchain
    man            View the man page for a given command
    self           Modify the rustup installation
    set            Alter rustup settings
    completions    Generate tab-completion scripts for your shell
    help           Prints this message or the help of the given subcommand(s)

  DISCUSSION:
    Rustup installs The Rust Programming Language from the official
    release channels, enabling you to easily switch between stable,
    beta, and nightly compilers and keep them updated. It makes
    cross-compiling simpler with binary builds of the standard library
    for common platforms.

    If you are new to Rust consider running `rustup doc --book` to
    learn Rust.

```

#### <span id="rustup_commands">rustup 常用命令</span>

rustup 自我升级：
```shell
rustup self update
```

安装组件：
```shell
rustup component add 组件名
```

组件列表：
```shell
rustup componet list
```


具体查询 [Rustup 文档](https://github.com/rust-lang/rustup/blob/master/README.md) （[Rustup Book](https://rust-lang.github.io/rustup/)）


---

#### <span id="rust_about_cargo">Cargo 包管理器</span>

[Cargo](https://github.com/rust-lang/cargo) 是 Rust 的包管理器和构建系统工具。


##### <span id="rust_cargo_commands">Cargo 命令</span>

`cargo build`： 构建工程
`cargo run`： 运行工程
`cargo clean`： 清理 target 目录
`cargo new`： 创建一个 pakage
`cargo init`： 在一个存在的目录中创建 package
`cargo update`： 升级
`cargo install`： 安装一个 Rust 可执行程序到 `.cargo/bin` 目录中
`cargo clippy`： lint 工具
`cargo fmt`： 代码格式化
`cargo tree`： 查看第三方库的版本和依赖关系
`cargo bench`： 运行 benchmark（基准测试，性能测试）

---

## <span id="rust_lsp">Rust LSP</span>

Rust 官方语言服务是 [RLS](https://github.com/rust-lang/rls) （Rust Language Server）。


后来社区版的 [rust-analyzer](https://github.com/rust-lang/rust-analyzer) 被 Rust 「招安」成功，成为官方项目。

未来 原来的RLS会「退役」，而 rust-analyzer 将会成为官方的语言服务--「rls-2.0」。

rust-analyzer 需要标准库的源码，所以得把源码装上：
```shell
rustup component add rust-src
```

理论上装上 rust-analyzer 就能使用这个 Rust 语言服务了。而装 rust-analyzer 可以通过操作系统软件包管理器装，也能通过 VSCode 的插件「顺道」安装。


### <span id="rust_lsp_links">Rust LSP 相关链接</span>
[rust-analyzer 安装文档](https://rust-analyzer.github.io/manual.html#installation)


---

