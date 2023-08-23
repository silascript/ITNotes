---
aliases:
  - 
tags:
  - PL
  - python
  - pip
  - conda
created: 2023-08-18 19:44:52
modified: 2023-08-22 17:31:26
---
# Python 笔记

---

## 目录

* [安装和常用](#安装和常用)
* [pip](#python_pip)
	* [更新](#更新)
	* [pip 换源](#pip%20换源)
* [conda](#python_conda)
	* [conda 安装](#python_conda_install)
	* [conda 换源](#python_conda_chsources)
	* [conda 常用命令](#python_conda_commands)
	* [conda 环境](#python_conda_environment)
* [基础语法](#python_syntax_basic)
	* [变量和表达式](#python_syntax_basic_varexp)
* [进阶语法](#python_syntax_advanced)
* [风格规范](#python_style)
* [文档](#python_resource)
	* [相关文档](#python_resource_doc)
	* [相关网站](#python_resource_links)
* [Python 教程视频](#python_course_videos)

---

## <span id="python_install">安装</span>

---

## <span id="python_pip">pip</span>

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

```shell
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
pip 的配置文件是放在 `.pip` 目录下的 pip.conf 文件中 (windows 是 pip.ini 文件)

示例：

```config
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

> [!tip]
> 
> 现在的 pip，配置文件是放在 `~/.config/pip/` 目录中。在 [conda](#python_conda) 中配置 pip 也是放在这个目录中。
> 

### pip 搜索

由于 `pip search` 命令不能用，所以使用「pip-search」这个包来实现搜索功能。
```shell
# 安装 pip-search
pip install pip-search

# 使用 pip-search
# 使用这货时，得敲 pip_search 命令，而不是 pip-search
pip_search 要搜索的包

```

### pip 问题

#### 虚拟环境外使用 pip

新版本的 linux 发生版，避免 Python 包管理器与系统底层冲突，所以禁止 `pip install`。只能在「虚拟环境」中使用 `pip`。如果执行 `pip install xxx`，会报 `externally managed environment` 错误提示。

当然不怕死的，可以使用 `--break-system-packages` 这个选项，硬装。

### pip 小工具

#### pipdeptree

`pipdeptree` 这个工具可以显示 pip 中各模块依赖「关系树」。有了这工具，删除模块时就可以更有「自信」了。

```shell
pip install pipdeptree
```

#### pip-autoremove

`pip-autoremove` 这工具在删除某模块时，把依赖的模块也一起「清理」了！

```shell
pip install pip-autoremove
```

---

## <span id="python_conda">conda</span>

[Conda](https://docs.conda.io/en/latest/index.html) [![conda repo](https://img.shields.io/github/stars/conda/conda?style=social)] (https://github.com/conda/conda) 分为 annaconda 和 miniconda 两个。

anaconda 是包含了一些常用包，并且有图形用户界面，属于比较完善的环境管理工具。

[miniconda](https://docs.conda.io/en/latest/miniconda.html) 是 anaconda 的精简版本，仅包含 conda 主程序和基本包，没有用户界面。

### <span id="python_conda_install">conda 安装</span>

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

使用 `sudo sh xxxxx.sh` 命令来安装 miniconda。
> [!tip] sh 执行权限
> 执行安装脚本时，最好还是加上 `sudo`，因为如果你要将 miniconda 安装在如 `/opt/miniconda` 目录下时，`miniconda` 这个自定义的 miniconda 安装目录如果不存在，安装脚本在安装到指定 [安装目录](#^774c11) 这一步骤时，会提示安装目录不存在，需要创建，这时就需要在 `opt` 下创建 `miniconda` 子目录。但 miniconda 安装脚本是不允许事先先建好个安装目录的，这个安装目录必须根据用户指定设置后，由脚本自行创建，如果指定了事先存在的安装目录，就会出现 `ERROR: File or directory already exists: '/opt/miniconda3'` 这样的提示，而由脚本自行创建，那就需要 root 权限，这同样也是为什么 miniconda 安装脚本默认安装路径是用户根下了，因为安装在用户根下不需要 root 权限就能创建安装目录，所以需要将 miniconda 安装到非用户目录下时，就得在执行安装脚本时使用 `sudo` 来执行。
^1f5cab

安装脚本执行过程： ^2156b8
1. 看许可并同意
2. 指定安装路径
> [!tip] 默认路径及指定路径
> 默认安装路径会是在 `/home/用户名/miniconda3`。可以自行指定路径。但如果想要将 miniconda 安装在非用户目录下，就需要在执行 miniconda 安装脚本时使用 `sudo`，以此来让安装脚本拥有 [root权限](#^1f5cab)，以便顺利创建 minconda 的安装目录。 ^774c11
3. 初始化
> [!info] 初始化做了什么
> ```shell
> Do you wish the installer to initialize Miniconda3
> by running conda init? [yes|no]
> [no] >>> yes
> no change     /opt/miniconda3/condabin/conda
> no change     /opt/miniconda3/bin/conda
> no change     /opt/miniconda3/bin/conda-env
> no change     /opt/miniconda3/bin/activate
> no change     /opt/miniconda3/bin/deactivate
> no change     /opt/miniconda3/etc/profile.d/conda.sh
> no change     /opt/miniconda3/etc/fish/conf.d/conda.fish
> no change     /opt/miniconda3/shell/condabin/Conda.psm1
> no change     /opt/miniconda3/shell/condabin/conda-hook.ps1
> no change     /opt/miniconda3/lib/python3.10/site-packages/xontrib/conda.xsh
> no change     /opt/miniconda3/etc/profile.d/conda.csh
> modified      /root/.bashrc
>
> ```
> 其实就是装了些基础包及配置了下环境变量。

#### <span id="python_conda_install_path">关于环境变量</span>

```config
__conda_setup="$('/home/silascript/miniconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/silascript/miniconda3/etc/profile.d/conda.sh" ]; then
        . "/home/silascript/miniconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/silascript/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup
```

在 `.profile` 或指定 shell 配置文件，如 zsh 的 `.zshrc` 中，加入以上配置，那终端启动一个 shell，这时就自动处于 conda 的一个虚拟环境中 -- 默认是处于 Base 环境。

> [!info]
> 
> 这不是单纯的像 `export PATH=$PATH:"$HOME/miniconda3/bin"`，这样将 bin 目录加入 Path 路径，这种添加环境变量的方式，不能进行 `activate` 激活切换环境。

---

### <span id="python_conda_chsources">conda 换源</span>

#### 生成 conda 配置文件

以清华源为例：

刚装完的 conda，是没有 `.condarc` 配置文件的，可以执行以下命令，生成 `.condarc` 文件：
```shell
conda config --set show_channel_urls yes
```

使用命令方式添加源：

```shell
conda config --add https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
```

在生成的 `.condarc` 文件中手动追加以下配置：

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

> [!tip]
> 
> 最好把配置文件中 `default_channels` 项中的 `default` 去掉，不然在每次安装软件时，都会到默认的镜像源里找。
> 
> 还有，网上一些教程，那些主 channel，是 `free` 的，这个 channel 的 python 太老了，不要用了，换成 `main`。

---

### <span id="python_conda_commands">conda 常用命令</span>

#### <span id="python_conda_commands_remove">删除</span>

#### <span id="python_conda_commands_create">创建</span>

创建命令 `create` 用于 [创建环境](#python_conda_environment_create)。更详细参数或选项可以通过 `conda create -h` 来查看。

#### <span id="python_conda_commands_list">List</span>

`conda list` 命令默认是列表出当前环境中包。 ^b4c89c

而如果什么环境都没有 [启动](#启动环境)，那就会列出默认的 [base 环境](#base%20环境) 中的包。

#### <span id="python_conda_commands_env">env</span>

`conda env` 是针对 [环境](#环境) 的命令。

> [!tip] conda env list
> `conda env list` 不能与 `conda list env` 混淆。
> 
> `conda env list` 是列出有哪些环境；而 `conda list env` 这种语法根本就是错误的，要 [列出某环境中的包](#环境包列表)，得使用 `conda list -n 环境名` 这样的命令。

---

### <span id="python_conda_environment">conda 环境</span>

#### base 环境

[安装](#^2156b8) 完 conda 后，conda 就自带了一个叫「base」的环境，这下环境是装在 conda 安装目录下的 `env` 子目录中。

#### <span id="python_conda_environment_create">创建环境</span>

创建环境使用到了 [创建](#python_conda_commands_create) 命令：`conda create -n myenv`
> [!info] 命令解释
> 使用 `-n` 参数来创建环境，实参的值就是自行指定的「环境名称」
> 
> 创建的环境会保存在用户目录下的 `.conda/envs` 目录下的同名目录中，如示例中的 *myenv* 环境，就会放在 `.conda/envs/myenv` 中。
> 
> 创建环境时，如不指定 Python 的版本，默认安装最新的 Python 版本。

创建时指定 Python 版本号及安装的包，其语法：`conda create -n your _env_name package_name python=X.X`

示例：
`conda create -n myenv numpy matplotlib python=3.8`

只指定 Python 版本，在创建环境时，也连带 [pip](#pip) 等也一并安装了。

#### <span id="python_conda_environment_removeqn">删除环境</span>

删除环境使用到了 [删除](#python_conda_commands_remove) 命令：`conda remove -n 环境名称 --all`
> [!info] 命令解释
> `--all` 指的是删除这个环境中所有的包

#### <span id="python_conda_enviroment_copy">复制环境</span>

复制环境语法：

```shell
conda create -n 新环境名 --clone 旧环境名
```

> [!tip] 复制环境
> 复制环境实质还是一种 [创建环境](#创建环境) 的「变体」。所以同样使用到 [创建](#创建) 命令。
> 	与普通创建环境不同，多了个 `--clone` 参数选项，用于指定复制自哪个已有的 [环境](#python_conda_environment)。

---

#### <span id="python_conda_environment_activate">启动环境</span>

启动环境：`conda activate 环境名`

> [!tip] 指定 shell 类型
> 第一次启动环境时，会有提示让你指定使用的 shell 类型：`conda init shell类型`。然后重启 Terminal，这时才会启动环境成功。而一次启动环境就不用再指定 Shell 类型了。

^d508bb

> [!tip] 关闭启动 base 环境
> 默认情况，在 `init` 时 [指定shell类型](#^d508bb) 重启 [终端](../Linux/Linux_Note.md#终端) 后，每次启打开 [终端](../Linux/Linux_Note.md#终端)，都会自动启动 [base 环境](#base%20环境)，这会影响到正常的 Terminal 的使用，有点烦，所以为了解决这问题，就需要关闭自动启动 base 环境。可以使用以下这个命令实现：
> 
> `conda config --set auto_activate_base false`
> 
> 执行完全命令后，可以查看下 `.condarc` 文件，应该可以看到 `auto_activate_base: false` 这个配置项存在，这就证明关闭了自动启动 base 环境。

---

#### <span id="python_conda_environment_deactivate">退出环境</span>

退出当前环境，使用：`conda deactivate` 命令。

#### <span id="python_conda_environment_list">环境列表</span>

要查看当前 conda 中有哪些环境，可以使用 `conda env list` 或 `conda info --env` 来查看。

另外也可以通过以下命令查看所有的虚拟环境：

```shell
conda info --envs
#也可以用缩写形式：
conda info -e
```

#### <span id="python_conda_environment_packagelist">环境包列表</span>

每一个环境其实就是各种「Package」的集合，所以一个环境中根本需求会有不同的包。那查看当前环境都装了哪个包，就可以使用 `conda list -n 环境名`。

如果在没有启动任何环境情况下，只输入 `conda list`，那 conda 会列出 [base 环境](#base%20环境) 下的包。
> [!tip]
> [base 环境](#base%20环境) 是 conda 的默认环境，即便它没有被启动，它也拥有相当的「特权」。
> 
> 在没有启动任何环境时，`conda list` 等效于 `conda list -n base`。
> 
> 即 `conda list -n 环境名`，这个语法能使用任何环境上，即便没有 [启动环境](#启动环境)，也能查看指定环境中安装了哪些包。

conda 能装什么包，可以通过 [anaconda官网](https://anaconda.org/) 查询。

#### <span id="python_conda_environment_export">导入导出环境</span>

导出环境：

```shell
conda env export > environment.yml
```

使用 `yml` 创建环境：

```shel
conda env create -n env_name -f environment.yml
```

#### <span id="python_conda_environment_package">conda 中的包</span>

安装包：

`conda install 包名`

### <span id="python_conda_pip">conda 中的 pip</span>

先使用 `which pip` 来确认现在用的是哪个哪个环境的 pip。如果当前虚拟环境没装，就使用 `conda install pip` 装下。

使用当前环境中的 pip 配置国内源，跟非虚拟环境一样：

```shell
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

最后配置文件都是 `~/.config/pip/pip.conf`，也就是说 conda 中的 pip 配置是同用的。

> [!tip] 关于模块安装路径
> 
> 在 conda 中使用 pip 安装模块，其模块是安装在 `~/minicoda/envs/虚拟环境/bin/` 目录下的，所以只在 `activate` 此环境才能使用。
> 
> 而且这里的优先级高于 `.local/bin` 下，即高于非 conda 环境下或使用 [pipx](#python_pipx) 安装的同名模块。

---

## <span id="python_pipx">pipx</span>

[Site Unreachable](https://github.com/pypa/pipx) 是一个自动建立虚拟环境来使用 Python 应用的工具。

pipx 与其他相近工具的比较：[pipx comparisons](https://pypa.github.io/pipx/comparisons/)

> [!info] pipx 安装条件
> 
> pipx 需要 Python 3.6+ 才可以使用

`pipx --help`，可查看 pipx 相关信息：

```shell
Virtual Environment location is /home/silascript/.local/pipx/venvs.
Symlinks to apps are placed in /home/silascript/.local/bin.

optional environment variables:
  PIPX_HOME             Overrides default pipx location. Virtual Environments will be installed to
                        $PIPX_HOME/venvs.
  PIPX_BIN_DIR          Overrides location of app installations. Apps are symlinked or copied here.
  PIPX_DEFAULT_PYTHON   Overrides default python used for commands.
  USE_EMOJI             Overrides emoji behavior. Default value varies based on platform.

```

> [!info] 关于 pipx 两个重要的目录
> 
> 一个是 pipx 的为应用建立的虚拟环境目录，默认是在 `~/.local/pipx/venvs`。
> 
> 就算是在 [conda](#python_conda) 中使用其虚拟环境的 pip 安装的 pipx，这个 pipx 所装的模块，仍是存放在 `~/.local/pipx/venvs/` 目录下。
> 
> 另一个是 pipx 中应用执行的 `bin` 文件「链接文件」所在的目录，默认是在 `~/.local/bin` 下，这其实是跟 pip 一样的。
> 
> 同样的，在 [conda](#python_conda) 的虚拟环境中使用的 pipx 装的模块的可执行程序，同样也是放在 `~/.local/bin/`。而这个程序只是一个 `link` 文件。
> 
> 也就说，在 [conda](#python_conda) 中各虚拟环境用 pipx 装的相同模块，其可执行程序会出现同名冲突，会报：「already seems to be installed. 」的揭示，因为这个可执行程序是个 Link 文件，它可以指向不同虚拟环境，如果不同虚拟环境下装相同的模块，后来生成的这个模拟可执行的 Link 文件就会覆盖之前装的。
> 
> 同样的也就意味着，不同虚拟环境下使用 pipx 装的模块，只要有一个虚拟环境装了，就可以在其他虚拟环境中使用，除非，这个在虚拟环境将此模块删除，或此虚拟环境本身就被删除。
> 
> 
> 所以得出一个重要的结论：pipx 装的模块在当前用户下，「全局性」更强，适合安装一些跨虚拟环境的模拟，如各种 [LSP](../vim/LSP_Complete.md)。
> 
> 同时，因为 pipx 这种「穿透性」，也就意味着 pipx 没太大必要在 [conda](#python_conda) 的虚拟环境中安装那些非「全局性」的模块。

> [!tip] pipx 安装模块要求
> 
> pipx 只能装那些有「cli」的模块，对于那些纯库类型的模块，像 pynvim，就不能装。

### 安装模块

[pip](#pip) 和 `pipx` 默认都是从 [pypi](https://pypi.org/) 上安装包。

使用 pipx 安装模块或应用，跟 [pip](#pip) 差不多。

```shell
pipx install 应用名
```

使用 github 源码安装：

> [!example] 示例
> 
> ```shell
> pipx install git+https://github.com/psf/black.git
> ```

安装指定版本的模块：

```shell
pipx install package==version
```

#### PypiSearch

如果想要搜索模块，可以安装 [pypisearch](https://github.com/shidenko97/pypisearch)：

```shell
pipx install pypisearch
```

安装完 [pypisearch](https://github.com/shidenko97/pypisearch) 后，就可以使用其搜索模块：

```shell
pypisearch 模块名
```

### 查询模块信息

列出已安装的所有模块：

```shell
pipx list
```

查看某模块的虚拟环境用了哪些包：

```shell
pipx runpip 模块名 list
```

### 运行

```shell
pipx run 模块名
```

### 升级

```shell
pipx upgrade 模块名
```

升级所有模块：

```shell
pipx upgrade-all
```

### 卸载

卸载域模块：

```shell
pipx uninstall 模块
```

卸载所有模块：

```shell
pipx uninstall-all
```

---

## <span id="python_packages">常用包</span>

### <span id="python_packages_jieba">结巴分词</span>

[结巴分词](https://github.com/fxsjy/jieba) 是一个 Python 的中文分词组件。

Obsidan 中 [中文分词插件](../Obsidian/Obsidian_Note.md#obn_plugins_wordsplitting_ch) 就有可能用到这个组件。

---

## <span id="python_syntax_basic">基础语法</span>

### <span id="python_syntax_basic_varexp">变量表达式</span>

#### 变量名规则

* 变量名只能包含字母、数字和下划线。不能以数字开头。
* 不能包含空格
* 不要用 Python 关键字和函数名或保留字作变量名

> [!tip] 
>
>慎用小写字母**l**和大写字母**O**，因为可能与数字**1**和**0**混淆。

### <span id="python_syntax_basic_type">简单数据类型</span>

#### <span id="python_syntax_basic_">数值类型</span>

##### <span id="python_syntax_basic_int">整型</span>

##### <span id="python_syntax_basic_float">浮点型</span>

##### <span id="python_syntax_basic_complex">复数</span>

#### <span id="python_syntax_basic_">字符串</span>

### 字典和集合

### 函数

### 类

### 文件

---

## <span id="python_syntax_advanced">进阶语法</span>

### 多任务

### 网络

### 图形界面

---

## <span id="python_style">风格规范</span>

### <span id="python_style_pep8">PEP8</span>

#### PEP8 相关链接

* [PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)
* [PEP 8 中文](https://blog.csdn.net/ratsniper/article/details/78954852)
* [PEP 257 – Docstring Conventions | peps.python.org](https://peps.python.org/pep-0257/)

### <span id="python_style_google">Google 规范风格</span>

#### Google 规范相关链接

* [Google Python 风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)
* [Google Python 编码规范 中文版](https://www.runoob.com/w3cnote/google-python-styleguide.html)
* [Google Python 编码规范 英文版](https://google.github.io/styleguide/pyguide.html)

---

## <span id="python_resource">资源链接</span>

### <span id="python_resource_doc">相关文档</span>

* [Python3.x 官方中文文档](https://docs.python.org/zh-cn/3/)

### <span id="python_resource_links">相关网站</span>

* [Python中文网](https://www.cnpython.com/)

---

---

## <span id="python_course_videos">Python 教程视频</span>

[Python 视频清单](Python_Videos.md)

