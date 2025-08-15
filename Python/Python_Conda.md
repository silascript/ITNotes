---
aliases: []
tags:
  - python
  - conda
  - miniconda
  - conda-forge
  - miniforge
created: 2025-08-15 01:32:08
modified: 2025-08-15 20:51:36
---

# Conda 笔记

---

## conda

[Conda](https://docs.conda.io/en/latest/index.html) [![conda repo](https://img.shields.io/github/stars/conda/conda?style=social)] (https://github.com/conda/conda) 分为 annaconda 和 miniconda 两个。

anaconda 是包含了一些常用包，并且有图形用户界面，属于比较完善的环境管理工具。

[miniconda](https://docs.conda.io/en/latest/miniconda.html) 是 anaconda 的精简版本，仅包含 conda 主程序和基本包，没有用户界面。

### <span id="conda_install">conda 安装</span>

miniconda 对于一般需求而言装这个就够用了。

到 [miniconda 官网](https://docs.conda.io/en/latest/miniconda.html) 下载相应平台安装文件。

以 Linux 为例，miniconda Linux 版本其实就是一个「大」 [Shell](../Linux/Shell/Shell_Note.md) 脚本文件。

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
> 
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

#### <span id="conda_install_path">关于环境变量</span>

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

在 `.profile` 或指定 shell 配置文件，如 zsh 的 `.zshrc` 中，加入以上配置，那终端启动一个 shell，这时就自动处于 conda 的一个虚拟环境中 -- 默认是处于 Base 环境，conda 就「接管」了 Python。

> [!info]
> 
> 这不是单纯的像 `export PATH=$PATH:"$HOME/miniconda3/bin"`，这样将 bin 目录加入 Path 路径，这种添加环境变量的方式，不能进行 `activate` 激活切换环境。

> [!tip] 不同配置文件中配置 conda 的区别
> 
> 如果是配在 `.profile` 或 `xprofile` 中，激活或注释取消这种「接管」，需要重启或注销重新登录系统才能生效，而在 `.zshrc` 这种针对某个 shell 的配置文件中配置，就能 `source` 此配置文件后，立即生效！

---

### <span id="conda_chsources">conda 换源</span>

#### 显示已有源

```shell
conda config --show-sources
```

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

```text
channels:
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
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

> [!info] 关于 condarc 注意点
> 
>> [!info]
>> 
>> ```yaml
>> channels:
>>   - defaults
>> default_channels:
>>   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
>>   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
>>   - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
>>
>> ```
> `channels` 节点下那个 `defaults` 是引用 `default_channels` 这个节点定义的 channel 的。强调引用名称叫 `defaults`，不要少了**s**！
> 
>> [!info] 
>> 
>> ```yaml
>> Channels:
>>   - defaults
>>   - https://repo.anaconda.com/pkgs/main
>>
>>``` 
>
> 
> 网上一些教程，那些主 channel，是 `free` 的，这个 channel 的 python 太老了，不要用了，换成 `main`。-- [anaconda](https://www.anaconda.com/) 官方从 4.7 版本就已经移除了 free 这个 channel 了。
>
>> [!qoute]
>> 
>> [remove free channel](https://www.anaconda.com/blog/why-we-removed-the-free-channel-in-conda-4-7)
> 
>> [!info] 相关资料
>> 
>> [Conda相关资料](Python_Material.md#Conda)

> [!info] 关于 channel 配置
> 
> 观察发现清华源的配置中，`custom_channels` 中各类包都是 [mirrors.tuna.tsinghua.edu.cn/anaconda/cloud](https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud) 这个地址，所以可以使用 `channel_alias`「精简」下。另外，channels 各源在检索时是有优先级的，依次从上到下逐一检索，所以鉴于 conda-forge 的包又全又新，可以根据需要将其优先级提高仅限于 main。
> 

 完整配置可如下：

```yaml
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
channel_alias: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
show_channel_urls: true
```

> [!tip] 清缓存
> 
> 切换 channel 后，使用 `conda clean -i` 或 `conda clean -a` 来清理缓存。
> 
> 配完源，使用 `conda update --all` 更下，看能显示出刚配的**channel**，如果没有，就是配置文件存在问题。

##### channel

channel，通道，顾名思义，就是软件或叫模块的来源。

* `pkgs/main`：python 最基础的包
* `pkgs/r`：提供 R 语言使用的包
* `pkgs/msys2`：windows 下 msys2 相关的包

* `Conda-Forge`：由社区维护的最常用的 Channel，包含的包很全，更新也非常即时

`conda install python` 安装 python，会连带以下包：

```shell
  ca-certificates    anaconda/pkgs/main/linux-64::ca-certificates-2024.7.2-h06a4308_0 
  libgcc-ng          conda-forge/linux-64::libgcc-ng-14.1.0-h69a702a_1 
  libuuid            anaconda/pkgs/main/linux-64::libuuid-1.41.5-h5eee18b_0 
  openssl            anaconda/pkgs/main/linux-64::openssl-3.0.15-h5eee18b_0 
  pip                anaconda/pkgs/main/linux-64::pip-24.2-py312h06a4308_0 
  setuptools         anaconda/pkgs/main/linux-64::setuptools-72.1.0-py312h06a4308_0 
  wheel              anaconda/pkgs/main/linux-64::wheel-0.43.0-py312h06a4308_0 
```

#### 恢复官方默认源

```shell
conda config --remove-key channels
```

#### 常用镜像

[Anaconda 软件仓库镜像使用帮助 - MirrorZ Help](https://help.mirrors.cernet.edu.cn/anaconda/)

### <span id="conda_uninstall">conda 卸载</span>

查看 conda 环境：

```shell
conda info --envs
```

1. 将环境一个个地 `remove`（删除到只剩个 base 就行了）：

```shell
conda remove -n 环境名称 --all
```

2. 执行卸载脚本

`~/miniconda3/` 目录下有一个卸载脚本：`uninstall.sh`，执行下它，确认卸载对话中输入 `yes`，就开始卸载：

```shell

$ ./miniconda3/uninstall.sh 
Are you sure you want to remove /home/silascript/miniconda3 and all of its contents?
[no] >>> yes
Uninstalling conda installation in /home/silascript/miniconda3...
Running conda init --reverse...
modified      /home/silascript/.zshrc

==> For changes to take effect, close and re-open your current shell. <==

No action taken.
Removing environments...

CondaEnvironmentError: Cannot remove current environment. Deactivate and run conda remove again

```

第一次执行，有可能会卸载不了，准确来说是卸载不完全（可以从卸载信息看出，只是修改了 [Shell](../Linux/Shell/Shell_Note.md) 的配置文件，将 conda 相关的配置删除而已。这时 `~/miniconda/` 这个 conda 的安装目录都还存在），会报这样的信息：`CondaEnvironmentError: Cannot remove current environment. Deactivate and run conda remove again`，而且卸载信息也提示你「关闭重新再启一个新的终端」（`close and re-open your current shell`），新的终端查看 `~/miniconda3` 这个目录存在与否，因为卸载不完全，应该还是存在的，所以再执行一次 `uninstall.sh` 应该就可以了，正常卸载会出现下面类似的信息：

```shell
$ ./miniconda3/uninstall.sh
Are you sure you want to remove /home/silascript/miniconda3 and all of its contents?
[no] >>> yes
Uninstalling conda installation in /home/silascript/miniconda3...
Running conda init --reverse...
No action taken.
No action taken.
Removing environments...

Remove all packages in environment /home/silascript/miniconda3:


## Package Plan ##

environment location: /home/silascript/miniconda3


The following packages will be REMOVED:

  _libgcc_mutex-0.1-main
  _openmp_mutex-5.1-1_gnu
  anaconda-anon-usage-0.7.1-py313hfc0e8ea_100
  annotated-types-0.6.0-py313h06a4308_0
  archspec-0.2.3-pyhd3eb1b0_0
  boltons-25.0.0-py313h06a4308_0
  brotlicffi-1.0.9.2-py313h6a678d5_1
  bzip2-1.0.8-h5eee18b_6
  c-ares-1.19.1-h5eee18b_0
  ca-certificates-2025.2.25-h06a4308_0
  certifi-2025.7.14-py313h06a4308_0
  cffi-1.17.1-py313h1fdaa30_1
  charset-normalizer-3.3.2-pyhd3eb1b0_0
  conda-25.5.1-py313h06a4308_0
  conda-anaconda-telemetry-0.2.0-py313h06a4308_1
  conda-anaconda-tos-0.2.1-py313h06a4308_0
  conda-content-trust-0.2.0-py313h06a4308_1
  conda-libmamba-solver-25.4.0-pyhd3eb1b0_0
  conda-package-handling-2.4.0-py313h06a4308_0
  conda-package-streaming-0.12.0-py313h06a4308_0
  cpp-expected-1.1.0-hdb19cb5_0
  cryptography-45.0.3-py313h2ccb017_0
  distro-1.9.0-py313h06a4308_0
  expat-2.7.1-h6a678d5_0
  fmt-9.1.0-hdb19cb5_1
  frozendict-2.4.2-py313h06a4308_0
  icu-73.1-h6a678d5_0
  idna-3.7-py313h06a4308_0
  jsonpatch-1.33-py313h06a4308_1
  jsonpointer-2.1-pyhd3eb1b0_0
  krb5-1.21.3-h8a1dbc1_1
  ld_impl_linux-64-2.40-h12ee557_0
  libarchive-3.7.7-hfab0078_0
  libcurl-8.14.1-h31d0fb7_0
  libedit-3.1.20230828-h5eee18b_0
  libev-4.33-h7f8727e_1
  libffi-3.4.4-h6a678d5_1
  libgcc-ng-11.2.0-h1234567_1
  libgomp-11.2.0-h1234567_1
  libmamba-2.0.5-haf1ee3a_1
  libmambapy-2.0.5-py313hdb19cb5_1
  libmpdec-4.0.0-h5eee18b_0
  libnghttp2-1.57.0-h2d74bed_0
  libsolv-0.7.30-he621ea3_1
  libssh2-1.11.1-h251f7ec_0
  libstdcxx-ng-11.2.0-h1234567_1
  libuuid-1.41.5-h5eee18b_0
  libxcb-1.17.0-h9b100fa_0
  libxml2-2.13.8-hfdd30dd_0
  lz4-c-1.9.4-h6a678d5_1
  markdown-it-py-2.2.0-py313h06a4308_1
  mdurl-0.1.0-py313h06a4308_0
  menuinst-2.3.0-py313h06a4308_0
  ncurses-6.4-h6a678d5_0
  nlohmann_json-3.11.2-h6a678d5_0
  openssl-3.0.16-h5eee18b_0
  packaging-24.2-py313h06a4308_0
  pcre2-10.42-hebb0a14_1
  pip-25.1-pyhc872135_2
  platformdirs-4.3.7-py313h06a4308_0
  pluggy-1.5.0-py313h06a4308_0
  pthread-stubs-0.3-h0ce48e5_1
  pybind11-abi-5-hd3eb1b0_0
  pycosat-0.6.6-py313h5eee18b_2
  pycparser-2.21-pyhd3eb1b0_0
  pydantic-2.11.7-py313h06a4308_0
  pydantic-core-2.33.2-py313hc6f7160_0
  pygments-2.19.1-py313h06a4308_0
  pysocks-1.7.1-py313h06a4308_0
  python-3.13.5-h4612cfd_100_cp313
  python_abi-3.13-0_cp313
  readline-8.2-h5eee18b_0
  reproc-14.2.4-h6a678d5_2
  reproc-cpp-14.2.4-h6a678d5_2
  requests-2.32.4-py313h06a4308_0
  rich-13.9.4-py313h06a4308_0
  ruamel.yaml-0.18.10-py313h5eee18b_0
  ruamel.yaml.clib-0.2.12-py313h5eee18b_0
  setuptools-78.1.1-py313h06a4308_0
  simdjson-3.10.1-hdb19cb5_0
  spdlog-1.11.0-hdb19cb5_0
  sqlite-3.50.2-hb25bd0a_1
  tk-8.6.14-h993c535_1
  tqdm-4.67.1-py313h7040dfc_0
  truststore-0.10.0-py313h06a4308_0
  typing-extensions-4.12.2-py313h06a4308_0
  typing-inspection-0.4.0-py313h06a4308_0
  typing_extensions-4.12.2-py313h06a4308_0
  tzdata-2025b-h04d1e81_0
  urllib3-2.5.0-py313h06a4308_0
  wheel-0.45.1-py313h06a4308_0
  xorg-libx11-1.8.12-h9b100fa_1
  xorg-libxau-1.0.12-h9b100fa_0
  xorg-libxdmcp-1.1.5-h9b100fa_0
  xorg-xorgproto-2024.1-h5eee18b_1
  xz-5.6.4-h5eee18b_1
  yaml-cpp-0.8.0-h6a678d5_1
  zlib-1.2.13-h5eee18b_1
  zstandard-0.23.0-py313h2c38b39_1
  zstd-1.5.6-hc292b87_0



Downloading and Extracting Packages:

Preparing transaction: done
Verifying transaction: done
Executing transaction: - WARNING conda.core.link:run_script(1581): pre-unlink script failed for package defaults/linux-64::conda-anaconda-tos-0.2.1-py313h06a4308_0
consider notifying the package maintainer
done

```

再次重启一个终端，会发现 `~/miniconda3` 这个目录都不存在，这就是真卸载完了。然后再进行下一步。

3. 删除相关的环境变量配置

删除 `.profle`、`.xprofile`、`.bashrc` 或 `.zshrc` 等配置文件中 conda 相关的 配置。
> [!tip] 
> 
> 有可能，执行 `uninstall.sh` 时，连 conda 安装时生成的相关配置都删干净了 -- 类似以下这种初始化配置：
> 
> ```shell
> 
> # >>> conda initialize >>>
> # !! Contents within this block are managed by 'conda init' !!
> __conda_setup="$('/home/silascript/miniconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
> if [ $? -eq 0 ]; then
> 	eval "$__conda_setup"
> else
> 	if [ -f "/home/silascript/miniconda3/etc/profile.d/conda.sh" ]; then
> 		. "/home/silascript/miniconda3/etc/profile.d/conda.sh"
> 	else
> 		export PATH="/home/silascript/miniconda3/bin:$PATH"
> 	fi
> fi
> unset __conda_setup
> # <<< conda initialize <<<
>
> ```
> 

4. 删除 conda 相关的目录及配置文件

> [!info] 
> 
> 有可能像 `.conda` 目录及 `.condarc` 配置文件，在执行 `uninstall.sh` 时就已经删除了！

* `rm -rf ~/.conda`
* `rm -rf ~/.condarc`
* `rm- rf ~/.cache/conda`

> [!info] 相关资料
> 
> * [linux卸载conda环境\_千锋教育](http://www.mobiletrain.org/about/BBS/150422.html)
> * [miniconda uninstall | anaconda docs](https://www.anaconda.com/docs/getting-started/miniconda/uninstall)
> 

---

### <span id="conda_commands">conda 常用命令</span>

`conda info`：查看 conda 相关信息

```shell

$ conda info      
 active environment : base
active env location : /home/silascript/miniconda3
		shell level : 1
   user config file : /home/silascript/.condarc
populated config files : /home/silascript/.condarc
	  conda version : 25.5.1
conda-build version : not installed
	 python version : 3.12.11.final.0
			 solver : libmamba (default)
   virtual packages : __archspec=1=nehalem
					  __conda=25.5.1=0
					  __glibc=2.42=0
					  __linux=6.6.101=0
					  __unix=0=0
   base environment : /home/silascript/miniconda3  (writable)
  conda av data dir : /home/silascript/miniconda3/etc/conda
conda av metadata url : None
	   channel URLs : https://mirrors.pku.edu.cn/anaconda/pkgs/main/linux-64
					  https://mirrors.pku.edu.cn/anaconda/pkgs/main/noarch
					  https://mirrors.pku.edu.cn/anaconda/pkgs/r/linux-64
					  https://mirrors.pku.edu.cn/anaconda/pkgs/r/noarch
					  https://mirrors.pku.edu.cn/anaconda/cloud/conda-forge/linux-64
					  https://mirrors.pku.edu.cn/anaconda/cloud/conda-forge/noarch
	  package cache : /home/silascript/miniconda3/pkgs
					  /home/silascript/.conda/pkgs
   envs directories : /home/silascript/miniconda3/envs
					  /home/silascript/.conda/envs
		   platform : linux-64
		 user-agent : conda/25.5.1 requests/2.32.4 CPython/3.12.11 Linux/6.6.101-1-MANJARO manjaro/25.0.7 glibc/2.42 solver/libmamba conda-libmamba-solver/25.4.0 libmambapy/2.0.8
			UID:GID : 1000:1000
		 netrc file : None
	   offline mode : False

```

#### <span id="conda_commands_config">配置</span>

##### 配置命令

`conda config` 是 conda 配置命令。此组命令可以在终端中对 conda 进行配置。

* `conda config --show`：查看当前配置
* `conda config --show-sources`：查看当前所有的配置文件及其配置内容
* `conda config --set changeps1 true`：开启显示环境名称功能 #环境名
* `conda config --set changeps1 false`：关闭显示环境名称功能 ^4d4740
* `conda config --add channels`：添加 channel
* `conda config --remove channels`：移除 channel
* `conda config --show channels`：查看现有 channel
* `conda config --set channel_priority strict`：设置 channel 解析优先级

config 相关参数可以使用 `conda config --help` 查看。

##### 手动配置

conda 所有配置，都是在 `.condarc` 配置文件中保存（一般配置的是用户 `HOME` 目录下那个 `.condarc` 文件，即 `~/.condarc`。因为 conda 的安装目录下还有一个 `.condarc`，这个一般是不会动到它的）,所以可以手动对此文件进行编辑，同样可以达到配置 conda 的目的。

> [!info] 
> 
> 一般只有像执行 `conda update --all` 全部分升级时，会连 conda 本体也升级时，才用到 conda 安装目录下那个 `.condarc` 文件：
> 
> ```shell
> $ conda update --all  
> Channels:
> - defaults
> - conda-forge
> Platform: linux-64
> Collecting package metadata (repodata.json): done
> Solving environment: done
>
> ```
> 

#### <span id="conda_commands_remove">删除</span>

* `conda clean --all`：清理缓存和未使用的软件包
* `conda remove 环境名`：[删除环境](#删除环境)

#### <span id="conda_commands_create">创建</span>

创建命令 `create` 用于 [创建环境](#conda_environment_create)。更详细参数或选项可以通过 `conda create -h` 来查看。

#### <span id="conda_commands_list">List</span>

`conda list` 命令默认是列表出当前环境中包。 ^b4c89c

而如果什么环境都没有 [启动](#启动环境)，那就会列出默认的 [base 环境](#base%20环境) 中的包。

#### <span id="conda_commands_env">env</span>

`conda env` 是针对 [环境](#环境) 的命令。

> [!tip] conda env list
> 
> `conda env list` 不能与 `conda list env` 混淆。
> 
> `conda env list` 是列出有哪些环境；而 `conda list env` 这种语法根本就是错误的，要 [列出某环境中的包](#环境包列表)，得使用 `conda list -n 环境名` 这样的命令。

---

### <span id="conda_environment">conda 环境</span>

#### base 环境

[安装](#^2156b8) 完 conda 后，conda 就自带了一个叫「base」的环境，这下环境是装在 conda 安装目录下的 `env` 子目录中。

#### <span id="conda_environment_create">创建环境</span>

创建环境使用到了 [创建](#conda_commands_create) 命令：`conda create -n myenv`

> [!info] 命令解释
> 
> 使用 `-n` 参数来创建环境，实参的值就是自行指定的「环境名称」
> 
> 创建的环境会保存在用户目录下的 `.conda/envs` 目录下的同名目录中，如示例中的 *myenv* 环境，就会放在 `.conda/envs/myenv` 中。
> 
> 创建环境时，如不指定 Python 的版本，默认安装最新的 Python 版本。

创建时指定 Python 版本号及安装的包，其语法：`conda create -n your _env_name package_name python=X.X`

示例：
`conda create -n myenv numpy matplotlib python=3.8`

只指定 Python 版本，在创建环境时，也连带 [pip](Python_Note.md#python_pip) 等也一并安装了。

#### <span id="conda_environment_removeqn">删除环境</span>

删除环境使用到了 [删除](#conda_commands_remove) 命令：`conda remove -n 环境名称 --all`
> [!info] 命令解释
> `--all` 指的是删除这个环境中所有的包

#### <span id="conda_enviroment_copy">复制环境</span>

复制环境语法：

```shell
conda create -n 新环境名 --clone 旧环境名
```

> [!tip] 复制环境
> 复制环境实质还是一种 [创建环境](#创建环境) 的「变体」。所以同样使用到 [创建](#创建) 命令。
> 	与普通创建环境不同，多了个 `--clone` 参数选项，用于指定复制自哪个已有的 [环境](#python_conda_environment)。

---

#### <span id="conda_environment_activate">启动环境</span>

启动环境：`conda activate 环境名`

> [!tip] 指定 shell 类型
> 
> 第一次启动环境时，会有提示让你指定使用的 shell 类型：`conda init shell类型`。然后重启 Terminal，这时才会启动环境成功。而一次启动环境就不用再指定 Shell 类型了。

^d508bb

> [!tip] 关闭启动 base 环境
> 
> 默认情况，在 `init` 时 [指定shell类型](#^d508bb) 重启 [终端](../Linux/Linux_Note.md#终端) 后，每次启打开 [终端](../Linux/Linux_Note.md#终端)，都会自动启动 [base 环境](#base%20环境)，这会影响到正常的 Terminal 的使用，有点烦，所以为了解决这问题，就需要关闭自动启动 base 环境。可以使用以下这个命令实现：
> 
> `conda config --set auto_activate_base false`
> 
> 执行完全命令后，可以查看下 `.condarc` 文件，应该可以看到 `auto_activate_base: false` 这个配置项存在，这就证明关闭了自动启动 base 环境。

---

#### <span id="conda_environment_deactivate">退出环境</span>

退出当前环境，使用：`conda deactivate` 命令。

#### <span id="conda_environment_revision">重置环境</span>

使用 `conda list --revision` 查看可以「回滚」哪些版本。

每个版本后有个数字作为版本号。

使用 `conda install --revision 数字版本号` 来「回滚」，如 `conda install --revision 0`。最后就是确认「回滚」（`Proceed ([y]/n)? `），回滚就完成了！

#### <span id="conda_environment_list">环境列表</span>

要查看当前 conda 中有哪些环境，可以使用 `conda env list` 或 `conda info --env` 来查看。

另外也可以通过以下命令查看所有的虚拟环境：

```shell
conda info --envs
#也可以用缩写形式：
conda info -e
```

#### <span id="conda_environment_packagelist">环境包列表</span>

每一个环境其实就是各种「Package」的集合，所以一个环境中根本需求会有不同的包。那查看当前环境都装了哪个包，就可以使用 `conda list -n 环境名`。

如果在没有启动任何环境情况下，只输入 `conda list`，那 conda 会列出 [base 环境](#base%20环境) 下的包。
> [!tip]
> [base 环境](#base%20环境) 是 conda 的默认环境，即便它没有被启动，它也拥有相当的「特权」。
> 
> 在没有启动任何环境时，`conda list` 等效于 `conda list -n base`。
> 
> 即 `conda list -n 环境名`，这个语法能使用任何环境上，即便没有 [启动环境](#启动环境)，也能查看指定环境中安装了哪些包。

conda 能装什么包，可以通过 [anaconda官网](https://anaconda.org/) 查询。

#### <span id="conda_environment_export">导入导出环境</span>

导出环境：

```shell
conda env export > environment.yml
```

使用 `yml` 创建环境：

```shel
conda env create -n env_name -f environment.yml
```

#### <span id="conda_environment_package">conda 中的包</span>

#### 搜索包

搜索某模块可选版本，以 python 为例：

```shell
conda search --full --name python
```

#### 安装

安装语法：`conda install 包名`

如果要指定包的版本可以使用：`conda install 包名=版本号`

使用 `install` 语法，无论当前 [环境](#环境) 是否已经安装该包，都会为该环境安装上此包。

`install` 语法在不同情景下，有不同的理解：

* 如果已经安装了，就相当于重装安装去替代原有的
* 如果已安装的版本高于将要安装的版本，相当于「降级」
* 如果已安装的版本低于将要安装的版本，就相当于升级
> [!info] 
> 当然如果只是单纯是想要升级，也可以直接使用 [升级](#升级) 语法。

> [!important] 
> 
> 整体而言，`install` 语法就不是简单从字面上只是安装，而更准确应该是「更换」。

更换当前环境下某模块版本，仍以 python 为例：

```shell
conda install python=3.11.5
```

##### 指定 channel

语法：`conda install -c <channel> <software>`

> [!tip] 
> 
> `-c` 参数是指定使用哪个 [channel](#channel)，不使用此参数，默认使用 `default`。
> 
> 这在使用 `conda search ` 搜索时，结果最后一栏就是这个 channel 了。如下示例显示：
> 
> ```shell
> Loading channels: done
> # Name                       Version           Build  Channel             
> python                        3.11.0 h10a6764_1_cpython  conda-forge         
> python                        3.11.0 h582c2e5_0_cpython  conda-forge         
> python                        3.11.0      h7a1cb2a_2  anaconda/pkgs/main  
> python                        3.11.0      h7a1cb2a_3  anaconda/pkgs/main  
> python                        3.11.0 ha86cf86_0_cpython  conda-forge         
> python                        3.11.0 he550d4f_1_cpython  conda-forge         
> python                        3.11.1 h2755cc3_0_cpython  conda-forge         
> python                        3.11.2 h2755cc3_0_cpython  conda-forge         
> python                        3.11.2      h7a1cb2a_0  anaconda/pkgs/main  
> python                        3.11.3 h2755cc3_0_cpython  conda-forge         
> python                        3.11.3      h7a1cb2a_0  anaconda/pkgs/main  
> python                        3.11.3      h955ad1f_1  anaconda/pkgs/main  
> python                        3.11.4      h7a1cb2a_0  anaconda/pkgs/main  
> python                        3.11.4      h955ad1f_0  anaconda/pkgs/main  
> python                        3.11.4 hab00c5b_0_cpython  conda-forge         
> python                        3.11.5      h7a1cb2a_0  anaconda/pkgs/main  
> python                        3.11.5      h955ad1f_0  anaconda/pkgs/main  
> python                        3.11.5 hab00c5b_0_cpython  conda-forge         
> python                        3.11.6 hab00c5b_0_cpython  conda-forge         
> python                        3.11.7      h955ad1f_0  anaconda/pkgs/main  
> python                        3.11.7 hab00c5b_0_cpython  conda-forge         
> python                        3.11.7 hab00c5b_1_cpython  conda-forge         
> python                        3.11.8      h955ad1f_0  anaconda/pkgs/main  
> python                        3.11.8 hab00c5b_0_cpython  conda-forge         
> python                        3.11.9      h955ad1f_0  anaconda/pkgs/main  
> python                        3.11.9 hb806964_0_cpython  conda-forge         
> python                       3.11.10 hc5c86c4_0_cpython  conda-forge         
> python                       3.11.10 hc5c86c4_1_cpython  conda-forge         
> python                       3.11.10 hc5c86c4_2_cpython  conda-forge         
> python                       3.11.10 hc5c86c4_3_cpython  conda-forge         
> python                       3.11.10      he870216_0  anaconda/pkgs/main  
> python                       3.11.11 h9e4cc4f_1_cpython  conda-forge         
> python                       3.11.11      he870216_0  anaconda/pkgs/main  
> ```

示例 安装或升级 python 到指定版本，并指定使用的 [channel](#channel) 为 `conda-forge`：

```shell
conda install python=3.12.10 -c conda-forge
```

##### 升级

升级模块到最新版本，以 python 为 例：

```shell
conda update python
```

> [!info] 相关资料
> 
> [Conda 使用](Python_Material.md#Conda%20使用)

#### 强行升级

有时使用 `conda update --all` 或 `conda update conda` 升级时，会出现以下错误：

```shell
RemoveError: 'archspec' is a dependency of conda and cannot be removed from
conda's operating environment.
RemoveError: 'boltons' is a dependency of conda and cannot be removed from
conda's operating environment.
RemoveError: 'packaging' is a dependency of conda and cannot be removed from
conda's operating environment.
RemoveError: 'tqdm' is a dependency of conda and cannot be removed from
conda's operating environment.
```

这种错误，有可能是使用 [pip](Python_Note.md#python_pip) 安装或更新了，再使用 conda 更新，就会出现「冲突」。

解决方法：

简单粗暴，「强制升级」：

```shell
conda update --force conda
```

> [!note] 相关资料
>
> [问题及解决](Python_Material.md#问题及解决) 

### <span id="conda_pip">conda 中的 pip</span>

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
> 而且这里的优先级高于 `.local/bin` 下，即高于非 conda 环境下或使用 [pipx](Python_Note.md#python_pipx) 安装的同名模块。

### Conda 问题

#### 错误 1

```shell
Error while loading conda entry point: conda-libmamba-solver (libarchive.so.20: cannot open shared object file: No such file or directory)

CondaValueError: You have chosen a non-default solver backend (libmamba) but it was not recognized. Choose one of: classic
```

或者

```shell
Error while loading conda entry point: conda-libmamba-solver (libxml2.so.2: cannot open shared object file: No such file or directory)
```

这些错误的原因都是相同的，就是这些包必须来自同一通道。

解决方案：

```shell
conda install --solver=classic conda-forge::conda-libmamba-solver conda-forge::libmamba conda-forge::libmambapy conda-forge::libarchive
```

```shell
conda update conda --solver=classic
conda install mamba -n base -c conda-forge
```

#### 错误 2

```shell
CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://mirrors.pku.edu.cn/anaconda/cloud/conda-forge/linux-64/repodata.json>
```

连接超时，原因不明。据说将 `https` 改成 `http` 就能改善连接慢的的问题。

> [!info] 
> 
> 也有可能的原因是包损坏，所以使用 `conda clean --packages --tarballs` 清理损坏的包，可能就会正常了。

#### 错误 3

[conda](#conda) 进行 `conda update --all` 时，有时会出现以下提示信息：

```shell
==> WARNING: A newer version of conda exists. <==
    current version: 24.9.1
    latest version: 24.9.2

Please update conda by running

    $ conda update -n base -c defaults conda
```

即便根据提示执行 `conda update -n base -c defaults conda` 代码同样还是会提示有更新的版本，那么可以通过指定版本号来「升级」（`install`） [conda](#conda) 本身：

```shell
conda install -n base -c defaults conda=版本号
```

---

## Miniforge

[miniforge](https://github.com/conda-forge/miniforge) 是 [conda-forge](https://conda-forge.org) 的一个精简版本。而 conda-forge 是使用的 [conda-forge](https://anaconda.org/conda-forge) [channel](#channel) 的开源版本的「conda」。

> [!info] 
> 
>  [conda-forge](https://conda-forge.org) 与 [conda](#conda) 默认的 [channel](#channel) 是 anacoda 公司维护的，而 conda-forge 是社区维护的。
>  
>> [!quote] 
>> 
>> conda-forge is a community led collection of recipes, build infrastructure and distributions for the conda package manager.
>>
>  

miniforge 已经融合了 [mamba](https://github.com/mamba-org/mamba)，mamba 是使用 [C++](../C/CPP_Note.md) 重写的 conda 包管理器。所以 miniforge 的速度比传统的 [conda](#conda) 要快。

> [!tip] 
> 
> 本来还有个 `mambaforge`，与 `miniforge` 几乎一样，后 `mambaforge`「退役」，现在推荐使用 `miniforge`。

### Miniforge 安装

安装条件：[miniforge requirements and installers](https://github.com/conda-forge/miniforge#requirements-and-installers)

[安装过程信息](Python_Conda_Info.md#安装过程信息)

> [!tip] 
> 
> 可以看到默认 channel 用的是 `conda-forge`

#### 初始化

安装最后一步，会提示你是否需要初始化，如果 `yes`，就会把初始化配置加入相应的 `rc`，下面以 [Zsh](../Linux/Shell/Zsh_Note.md) 为例：

```shell
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/silascript/miniforge3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/silascript/miniforge3/etc/profile.d/conda.sh" ]; then
        . "/home/silascript/miniforge3/etc/profile.d/conda.sh"
    else
        export PATH="/home/silascript/miniforge3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<


# >>> mamba initialize >>>
# !! Contents within this block are managed by 'mamba shell init' !!
export MAMBA_EXE='/home/silascript/miniforge3/bin/mamba';
export MAMBA_ROOT_PREFIX='/home/silascript/miniforge3';
__mamba_setup="$("$MAMBA_EXE" shell hook --shell zsh --root-prefix "$MAMBA_ROOT_PREFIX" 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__mamba_setup"
else
    alias mamba="$MAMBA_EXE"  # Fallback on help from mamba activate
fi
unset __mamba_setup
# <<< mamba initialize <<<

```

当然也可以在安装时不马上初始化，可以手动初始化，到 miniforge 安装目录执行 `init` 命令，如 `~/miniforge3/bin/conda init`。

> [!tip] 
> 
> Miniforge 与 [conda](#conda) 不一样，它有两个「核」，所以会有 `conda` 和 `mamba` 两段初始化配置。

### Miniforge 配置

Miniforge 配置同样也用 `.condarc` 文件，跟 [conda](#conda) 一样。

查看配置可以执行：`conda config --show`

#### 生成配置文件

可以手动新建 `.condarc` 文件，也可以执行 `conda config` 

#### 显示 channel url

默认不会显示 [channel](#channel) 的 `url`,如果要显示 url 可以执行 `conda config --set show_channel_urls yes`

> [!tip] 
>
> `.condarc` 设置为：
> 
> ```shell
> channels:
 > - conda-forge
> show_channel_urls: true
> ```

#### 关闭自动激活 base 环境

一般来说，[安装](#Miniforge%20安装) 完后会自动激活 base 环境，但如果不想要自动激活 base 环境，可以执行 `conda config --set auto_activate_base false`。

#### 关闭虚拟环境名称显示

有时 [Shell](../Linux/Shell/Shell_Note.md) 已经使用了某些主题自带「探测」当前 conda 虚拟环境名称并显示，所以 Miniforge 本身自带的显示当前虚拟环境名称的功能可以关闭。

使用 `conda config --set changeps1 false` 这个命令，便可以将显示当前虚拟环境名称功能关闭，这跟 [conda](#python_conda) 是完全一样的。

#### 镜像

添加 conda-forge 镜像：

```shell
conda config --add channels https://mirror.nju.edu.cn/anaconda/cloud/conda-forge/
```

### Miniforge 卸载

Miniforge 卸载没有像 [conda](#conda) 一样有个 `uninstall.sh` 脚本，所以得手工一项项卸载。

1. 重置 rc。执行完得重开新的终端才能进行下一步

```shell
# Use this first command to see what rc files will be updated
conda init --reverse --dry-run
# Use this next command to take action on the rc files listed above
conda init --reverse
```

2. 删除 `base` 环境目录，其实就是 Miniforge 安装的根目录，默认是 `~/miniforge3`

```shell
CONDA_BASE_ENVIRONMENT=$(conda info --base)
echo The next command will delete all files in ${CONDA_BASE_ENVIRONMENT}
# Warning, the rm command below is irreversible!
# check the output of the echo command above
# To make sure you are deleting the correct directory
rm -rf ${CONDA_BASE_ENVIRONMENT}
```

3. 删除配置文件及相关目录。就是 `.condarc` 文件及 `~/.conda` 目录。

```shell
echo ${HOME}/.condarc will be removed if it exists
rm -f "${HOME}/.condarc"
echo ${HOME}/.conda and underlying files will be removed if they exist.
rm -fr ${HOME}/.conda
```

4. 删除缓存目录。如果存在 `~/.cache/conda` 这个缓存目录，也删除了。

5. 删除各种系统配置文件 `.bashrc`、`.zshrc`、`profile` 等中相关配置

就是以下这两段 [初始化](#初始化) 的配置，删掉，重新再 `source` 下相关的 [Shell](../Linux/Shell/Shell_Note.md) 的配置文件，这样才算是真正卸载干净。

```shell

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/silascript/miniforge3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/silascript/miniforge3/etc/profile.d/conda.sh" ]; then
        . "/home/silascript/miniforge3/etc/profile.d/conda.sh"
    else
        export PATH="/home/silascript/miniforge3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<


# >>> mamba initialize >>>
# !! Contents within this block are managed by 'mamba shell init' !!
export MAMBA_EXE='/home/silascript/miniforge3/bin/mamba';
export MAMBA_ROOT_PREFIX='/home/silascript/miniforge3';
__mamba_setup="$("$MAMBA_EXE" shell hook --shell zsh --root-prefix "$MAMBA_ROOT_PREFIX" 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__mamba_setup"
else
    alias mamba="$MAMBA_EXE"  # Fallback on help from mamba activate
fi
unset __mamba_setup
# <<< mamba initialize <<<

```

> [!tip] 
> 
> 卸载文档：[miniforge uninstall](https://github.com/conda-forge/miniforge#uninstall)

### Miniforge 使用

Miniforge 可以使用 `conda` 和 `mamba` 两个管理命令。不过还是推荐使用 `mamba`。无论速度还是一些细节的优化，明显比传统的 `conda` 更好。如使用 `search` 命令，搜索软件包，`mamba` 会显示进度条，而不是 `conda` 那种「转圈」，而且搜索结果列表在显示上也稍微改进了一些。

```shell
$ mamba search python
Getting repodata from channels...

conda-forge/noarch                                  21.7MB @ 310.0kB/s 1m:9.7s
conda-forge/linux-64                                45.8MB @ 361.7kB/s 2m:6.6s
 Name   Version   Build                               Channel     Subdir  
───────────────────────────────────────────────────────────────────────────
 python 3.9.9     h543edf9_0_cpython    (+  1 builds) conda-forge linux-64
 python 3.9.7     h49503c6_0_cpython    (+  4 builds) conda-forge linux-64
 python 3.9.6     h49503c6_0_cpython    (+  1 builds) conda-forge linux-64
 python 3.9.5     h49503c6_0_cpython                  conda-forge linux-64
 python 3.9.4     hffdb5ce_0_cpython                  conda-forge linux-64
 python 3.9.23    hc30ae73_0_cpython                  conda-forge linux-64

```

#### 配置相关命令

`mamba config` 的子命令虽然更简洁，但部分子命令的语义有点不清楚。

##### 配置文件相关

* `mamba config list` 相当于一部分的 `conda config --show-sources` 功能，它只显示了 `~/.condarc` 配置文件的内容，对比如下：

```shell
$ conda config --show-sources
==> /home/silascript/miniforge3/.condarc <==
channels:
  - conda-forge

==> /home/silascript/.condarc <==
changeps1: False
channels:
  - defaults
default_channels:
  - https://mirror.nju.edu.cn/anaconda/cloud/conda-forge/
show_channel_urls: True

```

```shell

$ mamba config list
channels:
  - defaults
  - conda-forge
default_channels:
  - https://mirror.nju.edu.cn/anaconda/cloud/conda-forge/
changeps1: false

```

* `mamba config sources`，这子命令，语义就存在误导性，一眼看起来以为是显示源，但事实是显示有几个配置文件：

```shell
$ mamba config sources
Configuration files (by precedence order):
~/.condarc
~/miniforge3/.condarc
```

> [!info] 
> 
> 这子命令同样也有 `conda config --show-sources` 一部分功能。也就是说 `mamba` 将 `conda config --show-sources` 这个命令拆成两个子命令：
> 
> * `mamba config list`
> * `mamba config sources`
>> [!tip] 
>> 
>> 要看已设置的所有项还得使用 `conda config --show`，也由此可见 `mamba` 命令并不能完全替代 `conda`。

#### 创建环境

`mamba` 与传统的 `conda` 创建环境一样，例：`mamba create -n devex python=3.12`

#### 激活环境

`mamba activate 环境名`，例：`mamba activate devex`

#### 列出所有环境

`mamba env list`

示例：

```shell
$ mamba env list
Name   Active  Path                                  
─────────────────────────────────────────────────────────
base   *       /home/silascript/miniforge3           
devex          /home/silascript/miniforge3/envs/devex

```

#### 查看环境

`mamba list`

#### 搜索软件

在某环境中使用 `search` 命令搜索指定的软件包，可以是 `conda search 软件包名` 或 `mamba search 软件包名`。

使用 `mamba` 来搜，如果 [channel](#channel) 配了 [镜像](#镜像)，并设为默认 channel，那它只会搜索这个镜像，而如果使用 `conda` 来搜，因为它不但读取了 `~/.condarc`，还读取了 Miniforge 安装目录中的 `.condarc`，而安装目录中的 `.condarc` 是配了 `conda-forge` 原始的地址，所以使用 `conda` 搜，会对这两个 channel 进行搜索，下面例子对比下便可知：

```
$ mamba search pipx  
Getting repodata from channels...

https://mirror.nju.edu.cn/anaconda/cloud/conda-forge/linux-64          Using cache
https://mirror.nju.edu.cn/anaconda/cloud/conda-forge/noarch          Using cache
conda-forge/linux-64                                        Using cache
conda-forge/noarch                                          Using cache
 Name Version Build                         Channel           Subdir  
───────────────────────────────────────────────────────────────────────
 pipx 1.7.1   pyhd8ed1ab_0    (+  1 builds) mirror.nju.edu.cn noarch  
 pipx 1.7.0   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.6.0   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.5.0   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.4.3   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.4.2   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.4.1   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.4.0   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.3.3   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.3.1   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.3.0   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.2.1   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.2.0   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 1.1.0   py310hff52083_2 (+ 20 builds) mirror.nju.edu.cn linux-64
 pipx 1.0.0   pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 0.17.0  pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  
 pipx 0.16.5  pyhd8ed1ab_0                  mirror.nju.edu.cn noarch  

```

```shell
$ conda search pipx 
Loading channels: done
# Name                       Version           Build  Channel             
pipx                          0.16.5    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                          0.16.5    pyhd8ed1ab_0  conda-forge         
pipx                          0.17.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                          0.17.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.0.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.0.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.1.0 py310hff52083_2  anaconda/cloud/conda-forge
pipx                           1.1.0 py310hff52083_2  conda-forge         
pipx                           1.1.0 py310hff52083_3  anaconda/cloud/conda-forge
pipx                           1.1.0 py310hff52083_3  conda-forge         
pipx                           1.1.0 py310hff52083_4  anaconda/cloud/conda-forge
pipx                           1.1.0 py310hff52083_4  conda-forge         
pipx                           1.1.0 py311h38be061_4  anaconda/cloud/conda-forge
pipx                           1.1.0 py311h38be061_4  conda-forge         
pipx                           1.1.0  py37h89c1867_1  anaconda/cloud/conda-forge
pipx                           1.1.0  py37h89c1867_1  conda-forge         
pipx                           1.1.0  py37h89c1867_2  anaconda/cloud/conda-forge
pipx                           1.1.0  py37h89c1867_2  conda-forge         
pipx                           1.1.0  py37h89c1867_3  anaconda/cloud/conda-forge
pipx                           1.1.0  py37h89c1867_3  conda-forge         
pipx                           1.1.0  py38h373033e_3  anaconda/cloud/conda-forge
pipx                           1.1.0  py38h373033e_3  conda-forge         
pipx                           1.1.0  py38h373033e_4  anaconda/cloud/conda-forge
pipx                           1.1.0  py38h373033e_4  conda-forge         
pipx                           1.1.0  py38h578d9bd_1  anaconda/cloud/conda-forge
pipx                           1.1.0  py38h578d9bd_1  conda-forge         
pipx                           1.1.0  py38h578d9bd_2  anaconda/cloud/conda-forge
pipx                           1.1.0  py38h578d9bd_2  conda-forge         
pipx                           1.1.0  py38h578d9bd_3  anaconda/cloud/conda-forge
pipx                           1.1.0  py38h578d9bd_3  conda-forge         
pipx                           1.1.0  py38h578d9bd_4  anaconda/cloud/conda-forge
pipx                           1.1.0  py38h578d9bd_4  conda-forge         
pipx                           1.1.0  py39h4162558_3  anaconda/cloud/conda-forge
pipx                           1.1.0  py39h4162558_3  conda-forge         
pipx                           1.1.0  py39h4162558_4  anaconda/cloud/conda-forge
pipx                           1.1.0  py39h4162558_4  conda-forge         
pipx                           1.1.0  py39hf3d152e_1  anaconda/cloud/conda-forge
pipx                           1.1.0  py39hf3d152e_1  conda-forge         
pipx                           1.1.0  py39hf3d152e_2  anaconda/cloud/conda-forge
pipx                           1.1.0  py39hf3d152e_2  conda-forge         
pipx                           1.1.0  py39hf3d152e_3  anaconda/cloud/conda-forge
pipx                           1.1.0  py39hf3d152e_3  conda-forge         
pipx                           1.1.0  py39hf3d152e_4  anaconda/cloud/conda-forge
pipx                           1.1.0  py39hf3d152e_4  conda-forge         
pipx                           1.1.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.1.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.1.0    pyhd8ed1ab_5  anaconda/cloud/conda-forge
pipx                           1.1.0    pyhd8ed1ab_5  conda-forge         
pipx                           1.2.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.2.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.2.1    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.2.1    pyhd8ed1ab_0  conda-forge         
pipx                           1.3.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.3.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.3.1    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.3.1    pyhd8ed1ab_0  conda-forge         
pipx                           1.3.3    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.3.3    pyhd8ed1ab_0  conda-forge         
pipx                           1.4.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.4.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.4.1    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.4.1    pyhd8ed1ab_0  conda-forge         
pipx                           1.4.2    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.4.2    pyhd8ed1ab_0  conda-forge         
pipx                           1.4.3    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.4.3    pyhd8ed1ab_0  conda-forge         
pipx                           1.5.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.5.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.6.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.6.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.7.0    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.7.0    pyhd8ed1ab_0  conda-forge         
pipx                           1.7.1    pyhd8ed1ab_0  anaconda/cloud/conda-forge
pipx                           1.7.1    pyhd8ed1ab_0  conda-forge         
pipx                           1.7.1    pyhd8ed1ab_1  anaconda/cloud/conda-forge
pipx                           1.7.1    pyhd8ed1ab_1  conda-forge        
```

#### 安装软件

示例：

```shell
mamba install pipx
```

---

## conda 文档

* [Anaconda 中文网](https://anaconda.org.cn/)
* [conda-forge 文档](https://forge.conda.org.cn/docs/)
* [Mamba  documentation](https://mamba.readthedocs.io/en/latest/)

---

## 相关笔记

* [Python 笔记](Python_Note.md)
* [Conda 相关信息](Python_Conda_Info.md)
* [Python 资料清单](Python_Material.md)

