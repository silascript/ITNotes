---
aliases: []
tags:
  - PL
  - ruby
created: 2023-08-18 19:44:52
modified: 2024-09-12 20:51:28
---
# Ruby 笔记

---

## 目录

* [安装](#ruby_install)
	* [安装及包管理器](#ruby_install_managers)
		* [Ruby-Install](#Ruby-Install)
		* [Chruby](#Chruby)
		* [rbenv](#rbenv)
		* [ruby-build](#ruby-build)
		* [Frum](#Frum)

* [Gem](#ruby_gem)

---

## <span id="ruby_install">安装</span>

### Linux 下安装 Ruby

Linux 下使用各家的包管理器安装。

### Windows 下安装 Ruby

使用 [Scoop](https://scoop.sh/) 来安装：

```shell
scoop install ruby
```

### <span id="ruby_install_managers">安装及包管理器</span>

#### Ruby-Install

[ruby-instal](https://github.com/postmodern/ruby-install) 是一个 ruby 的安装工具。

安装默认最新稳定版：

```shell
ruby-install ruby
```

> [!info] 安装位置
> 
> 默认会将 ruby 装在 `~/.rubies/ruby-x.x.x` 上。`x.x.x` 是 ruby 的版本号，也就是说它会按版本号来分隔不同的安装目录。
> 
> ruby-install 是下载源码编译安装的。源码是下载到 `~/src` 目录。
> 
> ruby-install 缓存位置：`~/.cache/ruby-install`。

##### 其他安装命令

安装最新版：

```shell
ruby-install --update ruby
```

指定安装版本：

```shell
ruby-install ruby 3.1
```

指定安装路径：

```shell
ruby-install --install-dir /path/to/dir ruby
```

指定 `rubies` 「默认」路径：

```shell
ruby-install --rubies-dir /path/to/rubies/ ruby
```

> [!tip] rubies 路径
> 
> 默认情况，`rubies` 路径是在用户目录下，即 `~/.rubies`。而通过 `rubies-dir` 就可以指定将要安装的各版本 ruby 的「默认路径」。
> 
> 这跟上面那个 `--install-dir` 不同，`--install-dir` 是完全自由的，想指定哪，你将要装的 ruby 就放哪。而如果以后别的版本的 ruby 安装，如果没有继续使用 `--install-dir` 参数来指定，将会装在默认路径上。

将 ruby 装在 `/usr/local` 下：

```shell
ruby-install --system ruby 3.1.2
```

##### 指定下载源

另外，ruby-install 默认安装是下载源码安装，由于官方源速度有点慢，可以使用中国官方镜像：[https://cache.ruby-china.com](https://cache.ruby-china.com) 来提速。这里就得使用到 `-M` 选项了。

> [!example] 示例
> 
> ```shell
> # 默认安装最新稳定版
> # 使用了ruby-china的镜像
> ruby-install ruby -M https://cache.ruby-china.com/pub/ruby 
> # 指定安装的版本
> ruby-install ruby -M https://cache.ruby-china.com/pub/ruby 3.2.2 
> ```

##### 修改部分脚本

`/usr/share/ruby-install/` 目录是 ruby-install 的安装目录，下面有个 `ruby-version.sh` 的脚本文件，这个是到 github 上取 ruby 的版本号列表的。原来是 `https://raw.githubusercontent.com/postmodern/ruby-versions/master` 这个，但因为 `githubusercontent.com` 这个域名众所周知的缘故，所以访问上存在相当难度（它比 github.com 本身还要难，github 还能用 steam++，使用 "insteadOF" 修改.gitconfig，使用各种镜像替代），而这里的脚本是写死的，所以就算用加速站加速，也得改脚本。

修改后的：

```sh
ruby_versions_url="https://raw.gitmirror.com/postmodern/ruby-versions/master"
```

> [!tip]
> 
> gitmirror.com 是加速网址，可以到网上搜下。

##### ruby-install 相关文档

* 官方文档：[https://github.com/postmodern/ruby-install#readme](https://github.com/postmodern/ruby-install#readme)

#### Chruby

[chruby:](https://github.com/postmodern/chruby) 这个工具可能方便切换不同版本的 ruby。

使用系统包管理器安装。

默认安装在 `/usr/share/chruby/` 目录中，把 `/usr/share/chruby/chruby.sh` 加进 `.bashrc` 或 `.bash_profile` 或 `.profile` 等配置文件中就能用的。

```profile
source /usr/share/chruby/chruby.sh
```

切换版本：

```shell
chruby ruby-3.2.2
```

如果不带参数，`chruby` 会找当前目录中的 `.ruby-version` 文件中的版本号。

> [!tip]
> 
> `echo "3.2.2" > ./.ruby-version`
> 
> 或者 `echo "ruby-3.2.2" > ./.ruby-version`
> 

而 chruby 是没有「默认」版本的，只有使用到时，才敲下 `chruby` 切下。如果想要 shell 终端一启动就自动激活或者叫切换某 ruby 版本，那就把 `chruby` 写到配置文件（`.bashrc`、`.bash_profile` 或 `.profile`、`.zshrc` 等）中。

---

#### rbenv

[rbenv](https://github.com/rbenv/rbenv) 是一款 ruby 版本管理工具。

##### 安装

`yay -S rbenv`

##### 配置

装完后，执行下 `rbenv init`。会有以下揭示：

```shell
# Load rbenv automatically by appending
# the following to ~/.zshrc:
eval "$(rbenv init - zsh)"
```

```shell
# Load rbenv automatically by appending
# the following to ~/.bash_profile:
eval "$(rbenv init - bash)"
```

`rbenv init` 执行后，重启终端，会在 `~` 下多了一个 `.rbenv` 目录，这就是 rbenv 的根。默认这个目录下有两个子目录：`shims` 和 `versions`。

> [!info] 
> 
> `.rbenv` 下除了 `shims` 和 `versions` 两个目录，还存在一个默认情况下没有的目录，那就是 [plugins](#以%20[rbenv](%20rbenv)%20的插件方式安装) 目录，这个目录是用来装 `rbenv` 插件的。而 `rbenv` 最重要的一个插件就是 [ruby-build](#ruby-build)，没这个插件 `rbenv` 连下载 ruby 安装包都下不了。
> 
> 因为 `rbenv` 实质只是一个 ruby 版本切换器，本身不具备下载安装的功能，下载及安装功能都得通过别的工具实现，而其默认选项就是 `ruby-build`。

其中 `versions` 就是存放各个版本的 ruby。

这就是不同 shell 下添加不同的初始化代码。

##### 常用命令

* `rbenv commands`：列出 `rbenv` 所有命令
* `rbenv version`：显示当前使用的 ruby 的版本
* `rbenv versions`：显示已安装 ruby 的所有版本，前面带星号的，就是当前正在使用的版本。

要使用 `rbenv` 安装 ruby，如使用 `rbenv install -l ` 这个命令，显示下可以装哪些版本的 ruby，就必须安装 [ruby-build](#ruby-build) 这个插件。

* `rbenv install --list` 或 `rbenv install -l`：列出所有（全局）（global），或者称为「默认」可用的 ruby 版本。
* `rbenv install --list-all` 或 `rbenv install -L`：列出当前目录（`local`）所有已安装的 ruby 版本。
* `rbenv install 版本号`：安装指定的版本的 ruby。

 * `rbenv rehash` ：每当切换 ruby 版本和执行 bundle install 之后必须执行这个命令 
 * `rbenv which irb` ： 列出 irb 这个命令的完整路径 
 * `rbenv whence irb` ： 列出包含 irb 这个命令的版本

  * `rbenv global [版本号]`：显示或切换默认 ruby 版本
  * 
	  > [!tip]
	  > 
	  > 在使用 [ruby-build](#ruby-build) 安装完一个 ruby 后，就会有提示信息，如 `NOTE: to activate this Ruby version as the new default, run: rbenv global 3.2.2`，让你激活一个版本为默认的 ruby 版本。
	  > 
	  > 此命令版本号可选，如果此命令没有给出版本号，即为显示当前的默认版本。

##### 安装位置

`~/.rbenv/versions/` 这个目录下存各版本的 ruby，以 ruby 的版本号为子目录存放，如 `~/.rbenv/versions/3.2.2`，就是 `3.2.2` 这个版本的存放路径。

#### ruby-build

[rbenv](https://github.com/rbenv/ruby-build) 是 [rbenv](#rbenv) 的一个插件，专门用来下载安装 ruby 的。

##### 安装

安装 `ruby-build` 有两种方式：

###### 使用系统包管理器安装

使用系统的包管理器安装，如 `yay -S ruby-build`。

###### 以 [rbenv](#rbenv) 的插件方式安装

 [rbenv](#rbenv) 的插件方式安装：

```shell
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
```

> [!info]
> 
> 很简单，就是把 `ruby-build` 的东西下载到 `rbenv` 的插件目录中。
> 
> `rbenv root` 这个变量，默认是 `~/.rbenv`，也就是 rbenv 相关的东西都放在「根目录」。
> 
> 而 `rbenv root` 下的子目录 `plugins` 就是 rbenv 的插件存放目录。

安装完 `ruby-build`，就能使用诸如 `rbenv install -l ` 这类的的安装 ruby 的命令。

##### 配置

为了下载快点，所以得对 `ruby-build` 的镜像源进行一点配置。

在 `.bashrc` 或 `.bash_profile` 或 `.zshr` 或 `.profile` 文件中加入以下代码：

```config
export RUBY_BUILD_MIRROR_URL=https://cache.ruby-china.com
```

> [!tip] ruby-build 的配置项
> 
> [官方文档](https://github.com/rbenv/ruby-build#readme) Custom Build Configuration 一栏中就有列出 `ruby-build` 的配置项。
>
> 像 `RUBY_BUILD_HTTP_CLIENT`，这个配置选项还能配置使用什么下载工具下载 ruby，在 [aria2](../Linux/Linux_Note.md#inux_network_command_downloader_aria2)、[curl](../Linux/Linux_Note.md#curl%20常用操作) 和 [wget](../Linux/Linux_Note.md#wget%20主要操作) 三选一，这好像比 [Ruby-Install](#Ruby-Install) 牛一点。

##### 镜像插件

使用国内镜像插件来提速：[rbenv-china-mirror](https://github.com/AndorChen/rbenv-china-mirror)。

安装插件：

```shell
git clone https://github.com/andorchen/rbenv-china-mirror.git "$(rbenv root)"/plugins/rbenv-china-mirror
```

> [!tip]
> 
> 跟安装 [ruby-build](#ruby-build) 插件类似。
> 
> 而升级这看插件方式，跟升级普通 [Git](../Git/Git_Note.md) 项目一样，进到目录中 `git pull` 下就好了。
>
>```shell
> cd ~/.rbenv/plugins/rbenv-china-mirror
> git pull
> ```

但这个插件使用 [aria2](../Linux/Linux_Note.md#linux_network_command_downloader_aria2) 存在问题，所以换其他下载器。

在 `.bashrc` 或其他配置文件中加入以下代码：

```shell
export RUBY_BUILD_HTTP_CLIENT=wget
```

> [!info] rbenv 下载器可选项
> 
> `RUBY_BUILD_HTTP_CLIENT` 这个选项参数有三个可选：`aria2c`、`curl` 和 `wget`。
> 
> 并且对应的，还有 `RUBY_BUILD_ARIA2_OPTS`、`RUBY_BUILD_CURL_OPTS` 和 `RUBY_BUILD_WGET_OPTS`，三个选项可以让用户使用各下载器的选项参数。

##### rbenv 相关资料

* [rbenv readme](https://github.com/rbenv/rbenv#readme)
* [rbenv 使用指南 · Ruby China](https://ruby-china.org/wiki/rbenv-guide)

#### Frum

[frum](https://github.com/tako8ki/frum) 是一款使用 [Rust](../Rust/Rust_Note.md) 写的 ruby 版本管理器。

##### 安装及配置

可以使用系统的包管理器安装，如 [ArchLinux](../Linux/ArchLinux_Note.md)：

```
yay -S frum-bin
```

frum 有个配置目录，默认是在 `~/.frum`，可以使用 `echo $FRUM_DIR` 查看。

在 `.zshrc`，或其他配置文件中添加 `eval "$(frum init)"` 代码。

##### 常用命令

* `frum install -l`：列出可以安装的 ruby 版本。
* `frum install 版本号`：安装指定版本的 ruby。
> [!note] 
> 
> 默认使用 [cache.ruby-lang.org](https://cache.ruby-lang.org) 这个地址下载。
> 
> 下载的 ruby 会放安装在 `~/.frum/versions` 目录中，以版本号为子目录为区分。
>
> ```shell
>  ll .frum/versions 
> Permissions Size User       Group      Date Modified    Name
> drwxr-xr-x     - silascript silascript 2024-02-29 04:38  .
> drwxr-xr-x     - silascript silascript 2024-02-29 04:27  ..
> drwxr-xr-x     - silascript silascript 2024-02-29 04:59  .downloads
> drwxr-xr-x     - silascript silascript 2024-02-29 04:38  3.2.3
>
> ``` 

* `frum versions`：查看当前 ruby 版本情况，如下载了哪几个版本，正在应用哪个版本
 ```shell
 $ frum versions
 3.3.0
 * 3.2.3
 ```
 
* `frum local 版本号`：切换发前 ruby 版本，已安装的版本可以通过 `frum versions` 命令查看。
* `frum global 版本号`：指定全局 ruby 版本。
* `frum --ruby-build-mirror `：设定 ruby 下载镜像 url。单独使用没有意义，得配合 `install` 命令一起使用。
> [!example] 示例
> 
>`frum --ruby-build-mirror=https://cache.ruby-china.com/pub/ruby install 版本号`：使用指定镜像下载安装指定版本的 ruby。
>
> ```shell
> $ frum --ruby-build-mirror=https://cache.ruby-china.com/pub/ruby install 3.2.2
> ==> Downloading https://cache.ruby-china.com/pub/ruby/3.2/ruby-3.2.2.tar.xz
> ==> Extracting ruby-3.2.2.tar.xz
> ==> Building Ruby 3.2.2
> ```
> 只有在 `Downloading` 中显示出指定的镜像 url，才证明使用 `--ruby-build-mirror` 已经生效了。
> 
> 中国镜像地址：[https://cache.ruby-china.com/pub/ruby/](https://cache.ruby-china.com/pub/ruby/)
>
* `frum uninstall 版本号`：卸载指定版本的 ruby。

##### 相关资料

* [frum guide](https://mac.install.guide/ruby/14.html)
* [Installing Ruby with frum](https://www.jafeininger.de/writing_samples/ruby_with_frum/)
* [Comparison of version managers · rbenv/rbenv Wiki · GitHub](https://github.com/rbenv/rbenv/wiki/Comparison-of-version-managers)

---

## <span id="ruby_gem">Gem</span>

### <span id="ruby_gem_chsources">换源</span>

使用 [RubyGems 镜像](https://gems.ruby-china.com/) 来替换官方的源。

查看当前源：

```shell
gem sources -l
```

删除原来的源：

```shell
gem source -r https://rubygems.org/
```

添加新源：

```shell
gem source -r https://gems.ruby-china.com/
```

可以一行代码完成：

```shell
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
```

---

## <span id="ruby_links">Ruby 相关网站</span>

* [ruby官网](https://www.ruby-lang.org/)
* [ruby china](https://ruby-china.org/)
* [rubyinstaller.cn](https://rubyinstaller.cn/)