---
aliases: []
tags:
  - linux
  - ubuntu
  - vim
created: 2023-08-18 19:44:52
modified: 2023-11-25 21:11:31
---
# Ubuntu 笔记

---

## 目录

---

## <span id="ubuntu_chsource">换源</span>

Ubuntu 换源，其实就是是把 `/etc/apt/sources.list` 这个文件中的路径换了，具体操作请参考各镜像网站相关「帮助」。

各大常用开源国内都有多个镜像网站，比较出名的在以下这些：

[清华镜像](https://mirrors.tuna.tsinghua.edu.cn) 速度挺不错的源。具体换源请参考 [Ubuntu 镜像使用帮助](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)。

[中科大镜像](https://mirrors.ustc.edu.cn) 偶尔会抽风。关于 Ubuntu 换源的，请参考 [Ubuntu 源使用帮助](https://mirrors.ustc.edu.cn/help/ubuntu.html)。

[网易镜像](http://mirrors.163.com) 速度也挺不错的，就是版本有点旧，连 20.04 都没有。关于 Ubuntu 换源，请参考 [Ubuntu 镜像使用帮助](http://mirrors.163.com/.help/ubuntu.html)。

换完源， `apt-get update` 下！

---

## <span id="ubuntu_versions">Ubuntu 版本</span>

[查询系统版本](Linux_Note.md#查询系统版本)

Ubuntu 每六个月发布一个非 LTS 版本，每两年发布一个 LTS 版本，每个 LTS 有 5 年的维护时间。

* 主版本号为单数则表明此版本为短期支持版本，主要是为了测试一些新特性，测试 bug 等
* 主版本号为双数时表明此版本为长期支持板，一般有五年的维护时间。

> [!info] LTS
> LTS：Long Term Support

### ubuntu 历史各 LTS 版本清单

|       版本号       |       代号       |  发布时间  |
|:------------------:|:----------------:|:----------:|
|       22.04        | Jammy Jellyfish  | 2022-04-22 |
|     20.04 LTS      |   Focal Fossa    | 2020-04-23 |
|     18.04 LTS      |  Bionic Beaver   | 2018-04-26 |
|     16.04 LTS      |   Xenial Xerus   | 2016-04-21 |
|     14.04 LTS      |   Trusty Tahr    | 2014-04-18 |
|     12.04 LTS      | Precise Pangolin | 2012-04-26 |
| 11.04（Unity 桌面） |  Natty Narwhal   | 2011-04-28 |
|     10.04 LTS      |    Lucid Lynx    | 2010-04-29 |
|      8.04 LTS      |   Hardy Heron    | 2008-04-24 |
|      6.06 LTS      |   Dapper Drake   | 2006-06-01 |

---

## 问题与解决

### `The following packages have been kept back` 

原因：部分 packages 的安装版比 release 版新而出现的 `The following packages have been kept back` 问题。

解决方法：使用 `apt-get -u dist-upgrade` 统一更新到发布的版本。这命令会强制更新软件包到最新版本，并自动解决缺少的依赖包。

### `add-apt-repository: command not found`

`add-apt-repository` 命令是 **software-properties-common** 包的一部分，所以只要安装这个包就能用这个命令了。

```shell
sudo apt-get install software-properties-common
sudo apt-get update
```

### 安装 Vim 9

[vim 9]() 发布了，但 ubuntu 还没在官方仓库中加入。想要安装使用 vim 9 的，可以自行添加非官方 ppa 来安装 vim 9。

[非官方 Vim PPA](https://launchpad.net/~jonathonf/+archive/ubuntu/vim)

添加 ppa：
```shell
sudo add-apt-repository ppa:jonathonf/vim
sudo apt update

```

使用以下命令安装 vim：
```shell
suod apt install vim
```
如果之前已经装了 Vim，可以通过更新的方式更新到 9.0 版本：
```shell
sudo apt upgrade
```

查看 Vim 版本：
```shell
vim --version
```

如果想要使用回 Ubuntu 提供旧版（8.2）的 Vim，应先删除现有版本的 Vim，然后删除 PPA 后，再重新安装 Vim。
```shell
sudo apt remove vim
```
删除 PPA，否则你装的还是新版的 Vim：
```shell
sudo add-apt-repository -r ppa:jonathonf/vim
```

vim 具体使用请参考 [Vim_Note](../vim/Vim_Note.md) 笔记。

---

## 其他相关笔记

* [Linux 笔记](./Linux_Note.md)
* [Debian 笔记](./Debian_Note.md)
* [CentOS 笔记](./CentOS_Note.md)
* [Fedora 笔记](./Fedora_Note.md)
* [ArchLinux 笔记](./ArchLinux_Note.md)

