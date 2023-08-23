---
aliases: 
tags: PL ruby
created: 2023-08-18 19:44:52
modified: 2023-08-23 11:27:02
---
# Ruby 笔记

---

## 目录

* [安装](#ruby_install)

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

---

### rbenv

[rbenv](https://github.com/rbenv/rbenv) 是一款 ruby 版本管理工具。

#### ruby-build

[rbenv](https://github.com/rbenv/ruby-build) 是 [rbenv](#rbenv) 的一个插件，专门用来下载安装 ruby 的。

#### rbenv 相关文档

* [rbenv readme](https://github.com/rbenv/rbenv#readme)

* [rbenv 使用指南 · Ruby China](https://ruby-china.org/wiki/rbenv-guide)

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