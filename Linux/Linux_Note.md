# Linux 笔记

---
## 目录

* [配置文件](#linux_configs)

* [打包压缩](#linux_tarc)
  * [Tar 命令](#linux_tarc_tar)
  * [压缩工具](#linux_tarc_comptools)
    * [gzip](#linux_tarc_comptools_gzip)
    * [bzip2](#linux_tarc_comptools_bzip)
    * [compress](#linux_tarc_comptools_compress)
    * [XZ](#linux_tarc_comptools_xz)
* [字体](#linux_font)
* [网络](#linux_network)
  * [Linux 网络相关命令](#linux_network_command)
    * [ip](#linux_network_command_ip)
    * [curl](#linux_network_command_curl)
* [SSH 相关](#linux_ssh)
    * [安装与设置](#linux_ssh_insconf)
    * [SSH 各种问题](#linux_ssh_bugs)
* [Shell相关](#linux_shell)

* [部分软件安装设置](#linux_soft_installsetup)
  * [使用 desktop](#linux_soft_install_desktop)
---

## <span id="linux_configs">配置文件</span>

* profile 文件

  profile（`/etc/profile`） 文件用于设置系统级的环境变量和启动程序，在这个文件下配置会对所有用户生效。
  
  `~/.profile` 这是用户级别的环境变量配置文件。

  大多数发行版使用的是`~/.profile` 。因为`~/.profile` 文件可以被所有 Shell 读取，而 `~/.bash_profile` 仅能被 **Bash Shell** 读取。

* bashrc

  bashrc 只会被 bash shell 调用。

  bashrc 文件分为两种级别：
    * 系统级：`/etc/bashrc`
    * 用户级：`~/.bashrc`

  bashrc 是交互式非登录 Shell 时被执行。

* bash_profile 文件

  bash_profile 文件是作为交互式登录 Shell 被调用情况下，被读取执行。


在登录 Shell 的 bash 环境时，会依序执行以下三个文件其中一个：
* `~/.bash_profile`
* `~/.bash_login`
* `~/.profile`

 根据使用者账号，于其 `homt` 目录内读取 `~/.bash_profile` ；

 读取失败则会读取 `~/.bash_login` ；

 再次读取失败则读取 `~/.profile` 
 > 这三个文件设定基本无差别，仅读取上有优先关系

> 路径末尾不能以 **\/** 结尾，否则将导致整个 PATH 变量出错。



---

## <span id="linux_tarc">打包压缩</span>

Linux 下打包和压缩是分两部分的。

### <span id="linux_tarc_tar">Tar 命令</span>

Linux 下最常用的打包程序就是 tar。

tar 本身只能用来打包，不具备压缩能力。但 tar 可以与 gzip、bzip2等压缩工具结合一起来用。

tar 包以 **.tar** 为后缀名。

tar 常用选项和参数：

* -f（--file）：指定存档或设备中的文件（默认是 「-」，表示 标准输入/输出）
* -v （--verbose）：显示文件处理过程
* -t （--list）：查看存档中的文件列表
* -C （--directory）：提取存档到指定目录
* -c （--create）：创建一个新的存档，即「压缩」
* -x （--extract）：从存档提取文件，即「解压」
* -r （--append）：将文件附加到存档结尾，即增加文件到包中
* -u （--update）：仅将较新的文件附加到存档中，即更新
> -u 只会添加 mtime 初修改过的文件；-r 直接添加，不检查 mtime。

---

### <span id="linux_tarc_comptools">压缩工具</span>

#### <span id="linux_tarc_comptools_gzip">gzip</span>

gzip 是 GNU 组织开发的一个压缩程序，后缀名为 **.gz** 。

与 gzip 相对的解压的程序是 gunzip。

与 tar 配合使用，通过 **-z** 参数来调用。

使用示例：
```shell
# 打包并使用 gzip 压缩
tar -czvf xxx.tar.gz *.jpg

# 解压
tar -xzvf xxx.tar.gz

# 查看包内文件
tar -tzf xxx.tar.gz

# 如果加上v 可以看到包内文件的如权限等详细信息
tar -czvf xxx.tar.gz
tar -xzvf xxx.tar.gz

```


#### <span id="linux_tarc_comptools_bzip">bzip2</span>

**bzip2** 是一个压缩能力更强的压缩程序，后缀名为 **.bz2**。

与 bzip2 相对的解压程序是 **bunzip2**。

与 tar 配合使用，通过 **-j** 参数来调用。

使用示例：
```shell
tar -cjf xxx.tar.bz2 *.jpg

# 解压
tar -xjf xxx.tar.bz2

# 如果加上v 可以看到包内文件的如权限等详细信息
tar -cjvf xxx.tar.gz
tar -xjvf xxx.tar.gz
```

#### <span id="linux_tarc_comptools_compress">compress</span>

**compress** 也是一个压缩程序，不过用得比较少，后缀名为 **.Z**。

与 compress 相对的解压程序是 **uncompress**。

与 tar 配合使用，通过 **-Z** 参数来调用。


使用示例：
```shell
tar -cZf xxx.tar.Z *.jpg

# 解压
tar -xZf xxx.tar.Z

# 如果加上v 可以看到包内文件的如权限等详细信息
tar -cZvf xxx.tar.Z
tar -xZvf xxx.tar.Z
```


#### <span id="linux_tarc_comptools_xz">XZ Utils</span>

[XZ Utils](http://tukaani.org/xz/) 是使用 「LZMA」算法的无损数据压缩的压缩程序。

因为早期 xz 与 tar 没有整合，所以压缩和解压得分「两步走」。

常用选项：
* `-z,--compress`：压缩文件并删除被压缩文件
* `-d,--decompress`：解压并删除压缩包
* `-k,--keep`：压缩或解压时保留源文件
* `-0...9`：设置压缩等级，0~9，不设置默认为 6
* `-v`：压缩和解压过程详细信息

示例：

* 压缩
```shell
# 先 tar
tar -cvf xxx.tar xxx
# 压缩
xz -z xxx.tar

```
* 解压
```shell
# 解压 xz
xz -d xxx.tar.xz
# 如果保留压缩包
xz -dk xxx.tar.xz

# 解压 tar包
tar -xvf xxx.tar

```


与 tar 配合使用，通过 **-J** 参数来调用。
> 低版本的 tar 不支持，就得先 tar 再使用

示例：
```shell
# 压缩
tar -Jcf xxx.tar.xz *.jpg

# 解压
tar -Jxf xxx.tar.xz

```

---
## <span id="linux_font">字体</span>

安装字体：
1. 使用字体安装程序点击安装
> 也可以直接将字体文件复制到 `/usr/share/fonts` 目录下  
> 如果是 ttf 字体，应复制在 `/usr/share/fonts/TTF` 目录下  
> 也可以直接把字体文件复制到 `~/.local/share/fonts` 目录， 这是仅当前用户可用的字体

2. 刷新
```shell
fc-cache -fv
```

查看字体：
```shell
# 查看中文字体
fc-list :lang=zh
```


---

## <span id="linux_network">网络</span>


### 一些网络概念

#### 以太

**以太**是一种虚拟的物质，英文 Ether的音译，一般可以理解为一种看不见摸不着的物质。

最早提出「以太」概念是亚里士多德。

随着科学发展，「以太」理论也逐渐被科学界抛弃。

虽然以太理论已经基本被淘汰，但「以太」这词在使用中并未消亡，「以太网」就是其一。

以太网是一种计算机局域网技术，而IEEE 802.3 标准则是以太网技术被广泛应用之后制定的以太网技术标准。

以太网是目前应用最普遍的局域网技术。

也就是因为有此原因，所以 Linux 系统中网络接口都使用 「eth」为名称。

#### <span id="linux_network_bridge">网桥</span>

简单说，网桥就是连通两个本来处于隔离的网络的一个「桥梁」。

---

### <span id="linux_network_command">Linux 中网络相关命令和工具<span>

#### <span id="linux_network_command_ip">ip</span>

**ip** 是用来查询或维护网络相关的工具。

包括**路由**（Routing）、**网络设备**（Device）、**策略路由**（Policy Routing）以及**隧道**（Tunnel）。

**ip 常用命令**

* `ip link`：显示网络设备
* `ip addr` ：查看网络地址
    > 这个命令是把网络设置及网络地址、掩码都显示出来。比`ip link` 多显示一些信息。
* `ip linke show 网络设备名称`：显示指定网络设备的信息。
* `ip link set 网络设备 up`：启用网络设备
* `ip link set 网络设备 down`：禁用网络设备


#### <span id="linux_network_command_curl">curl</span>

**curl** 是一个用来对服务器发起请求并把响应输出到标准输出的工具。


---

### IPtables 

IPtables 中可以做各种网络地址转换，网络地址转换主要有两种：**SNAT** 和 **DNAT**。

**SNAT** 是 「source networkaddress translation」的缩写，即「源地址目标转换」。


**DNAT** 是 「destination networkaddress translation」的缩写，即「目标网络地址转换」。


---

## <span id="linux_ssh">SSH</span>

### <span id="linux_ssh_insconf">安装和设置SSH</span>

以 debian 系为例

#### 安装：

```shell
apt-get update
apt-get upgrade

apt-get install openssh-server
```

#### 设置：

对`/etc/ssh/sshd_config` 文件进行设置。

* 开启端口：`Port 22`

* 监听地址：`ListenAddress 0.0.0.0`

* 添加 `PermitRootLogin yes` 使用 root 登录的权限

* 将`PermitTTY` 设为 `yes`

* 将 `UsePAM` 设为 `no`

#### 重启
```shell
service ssh restart
```
> 查看 ssh 是否已开启，可以使用 `ssh -V` 命令。


#### 连接

使用 ssh 命令连接到远程服务器：

```shell
ssh 用户名@ip -p 端口
```

### <span id="linux_ssh_bugs">SSH 各种问题</span>

`Host key verification failed` 错误

解决方法：

方法1： 直接将 `.ssh/known_hosts` 文件删除即可。

方法2： 修改 `/etc/ssh/ssh_config` 配置文件中，设置 `StrictHostKeyChecking` 的值

默认情况下，`StrictHostKeyChecking=ask` 。出现提示，如果连接和 key 不匹配，给出提示，并拒绝登录。

`StrictHostKeyChecking=no` 是最不安全的级别。如果连接 server 的 key 在本地不存在，那么就自动添加到文件中（默认是 `.ssh/known_hosts` 文件中），并且给出一个警告。

`StrictHostKeyChecking=yes` 是最安全级别。如果连接与 key 不匹配，就拒绝连接，不会提示详细信息。


### <span id="linux_ssh_tools">SSH 工具</span>

#### <span id="linux_ssh_tools_putty">Putty</span>

这是款古老的 SSH 工具。

优点：小巧、免费。
缺点：设置不太人性化

#### <span id="linux_ssh_tools_electerm">Electerm</span>

[Electerm](https://electerm.html5beta.com) [![Electerm Repo](https://img.shields.io/github/stars/electerm/electerm?style=social)](https://github.com/electerm/electerm) 开源免费，而且 win、mac 和 linux 平台都能用。

其实 [Electerm](https://electerm.html5beta.com) 是能当终端使用的。


颜值还非常高：
![electerm](https://github.com/electerm/electerm-resource/raw/master/static/images/electerm.gif)



#### <span id="linux_ssh_tools_tabby">Tabby</span>

[Tabby](https://tabby.sh) [![Tabby Repo](https://img.shields.io/github/stars/Eugeny/tabby?style=social)](https://github.com/Eugeny/tabby) 同样是一款Win、Linux 和 Mac 都能用的开源 SSH 工具。之前的名字叫 「Terminus」。

其实这货更像是一个终端，只是集成了 SSH 功能。

这颜值比[Electerm](https://electerm.html5beta.com) 更高，功能更强。就是字体设置不太方便，还得手动输入字体名。

这货还能装插件，太过分了！

有一大堆的配色方案可选，真的非常过分了！

搭配插件，tabby 的配置能使用 gist 进行同步。





#### <span id="linux_ssh_tools_finalshell">FinalShell</span>

[FinalShell](https://www.hostbuf.com) 是一款 Win、Linux 和 Mac 都能用的 ssh 工具。

优点：功能强、设置顺手，所有平台都能用

缺点：耗内存、启动有点慢




---

## <span id="linux_shell">Shell 相关</span>

具体请查看[Shell 笔记](./Shell_Note.md)


---

## <span id="linux_soft_installsetup">部分软件安装设置</span>

---

### <span id="linux_soft_install_desktop">使用 desktop</span>

1. 新建 `desktop` 文件
在/usr/share/applications/目录中新建一个后缀名为desktop的文件
> 也可以在 .local/share/applications 目录新建 desktop 文件

2. 编辑 `desktop` 文件
> [Desktop Entry] (这里大小写敏感，写错一个就不会显示)  
> Type=Application 类型  
> Name=名称  
> Name[zh_CN]=简中名称  
> Name[zh_TW]=繁中名称  
> Comment=鼠标放在图标上时的提示文字  
> Comment[zh_CN]=与上类似不过是简体中文  
> Comment[zh_TW]=繁中的提示文字  
> Icon=图标路径  
> Exec=可执行文件的路径  
> Categories=程序的分类标签，多个标签使用;号分隔(Application;IDE;Development;Editor)  
> Terminal=false 是否使用启用终端  
> StartupNotify=true 



---

### <span id="linux_soft_install_komodo">Komodo Edit 安装</span>

1. 下载
```shell
wget https://downloads.activestate.com/Komodo/releases/12.0.1/Komodo-Edit-12.0.1-18441-linux-x86_64.tar.gz
```

2. 解压
```shell
tar -zxvf xxx.tar.gz
```

3. 安装

转到新解压的目录，开始安装：
```shell
sudo ./install.sh -I /opt/KomodoEdit
```

4. 添加 PATH 变量 

`.bashrc` 或 `.bash_profile` 文件中添加
```shell
export PATH=$PATH:/opt/KomodoEdit/bin
```
source `.bashrc` 或 `.bash_profile` 

5. 如果需要在终端中调，可以添加软链：
```shell
sudo ln -s /opt/KomodoEdit/bin/komodo /usr/local/bin/komodo
```




