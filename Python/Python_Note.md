---
aliases:
  - 
tags:
  - PL
  - python
  - pip
  - conda
created: 2023-01-13, 12:27:46
modified: 2023-01-30, 9:19:48
---
# Python 笔记

---

## 目录

* [安装和常用](#安装和常用)
	* [更新](#更新)
	* [pip 换源](#pip%20换源)
* [Python 教程视频](#python_course_videos)

---

## 安装和常用

### 更新

```python
# 列出可以更新的包
pip list --outdated

# 更新指定的包
pip install --upgrade xxx

# 更新pip
pip install --upgrade pip
# 或 使用python -m 来更新pip
python -m pip install --upgrade pip


```

### pip 换源

#### 临时换源并安装指定包

```python
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 包名
```

清华的镜像源每五分钟更新一次，大而全，推荐大家使用。
国内其他源:

* 清华：<https://pypi.tuna.tsinghua.edu.cn/simple>
* 阿里云：<http://mirrors.aliyun.com/pypi/simple/>
* 中国科技大学 <https://pypi.mirrors.ustc.edu.cn/simple/>
* 华中理工大学：<http://pypi.hustunique.com/>
* 山东理工大学：<http://pypi.sdutlinux.org/>
* 豆瓣：<http://pypi.douban.com/simple/>

#### 永久性换源

1. 第一种方式：

```python

# 清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
# 阿里源
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
# 腾讯源
pip config set global.index-url http://mirrors.cloud.tencent.com/pypi/simple
# 豆瓣源
pip config set global.index-url http://pypi.douban.com/simple/

```

2. 第二种方式：

直接修改 pip 配置文件
pip 的配置文件是放在.pip 目录下的 pip.conf 文件中 (windows 是 pip.ini 文件)
示例：

```config
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

---

## <span id="python_conda">conda</span>

[Conda](https://docs.conda.io/en/latest/index.html) [![conda repo](https://img.shields.io/github/stars/conda/conda?style=social)] (https://github.com/conda/conda) 分为 annaconda 和 miniconda 两个。

anaconda 是包含了一些常用包，并且有图形用户界面，属于比较完善的环境管理工具。

[miniconda](https://docs.conda.io/en/latest/miniconda.html) 是 anaconda 的精简版本，仅包含 conda 主程序和基本包，没有用户界面。

### <span id="python_conda_install">miniconda 安装</span>

miniconda 对于一般需求而言装这个就够用了。

到 [miniconda 官网](https://docs.conda.io/en/latest/miniconda.html) 下载相应平台安装文件。

以 Linux 为例，miniconda Linux 版本其实就是一个「大」 shell 脚本文件。

嫌官网速度慢，可以到清华镜像站下：[https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda](https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda) 。

也可以使用 [wget](https://www.gnu.org/software/wget/) [![wget repo](https://img.shields.io/github/stars/mirror/wget?style=social)](https://github.com/mirror/wget) 进行下载。
```shell
# -c 是能实现断点续传
wget -c https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
# -O 是能下载到用户目录后重命令文件
wget -c -O https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

更详细的 wget 的使用，可以参考 [wget 介绍](../Linux/Linux_Note.md#linux_network_command_downloader_wget)。

---

### <span id="python_conda_chsources">conda 换源</span>

#### 生成 conda 配置文件

以清华源为例：

```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes # 生成".condarc"文件
```

在生成的 `.condarc` 文件中追加以下配置：

```
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloudtext
```

---

## <span id="python_course_videos">Python 教程视频</span>

[Python 视频清单](Python_Videos.md)

