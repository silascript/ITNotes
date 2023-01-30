---
aliases: 
tags: PL ruby
created: 2023-01-13, 12:27:46
modified: 2023-01-30, 7:55:06
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

