
# CentOS 笔记

---
## 目录

* [yum 使用](#cent_yum_op)
* [常用软件安装和使用](#cent_cs_info)
  * [启用 SSH 功能](#cent_cs_ssh)
---

## <span id="cent_yum_op">yum 使用</span> 

```shell
  yum [options] [command] [package ...]

```

### <span id="cent_yum_cc">yum 常用命令</span>

* 列出所有可更新的软件列表：**yum check-update**
* 更新所有软件：**yum update**
* 仅安装指定软件：**yum install 软件包名**
* 仅更新指定软件：**yum update 软件包名**
* 列出所有可安装软件列表：**yum list**
* 删除软件：**yum remove 软件包名**
* 查找软件：**yum search 查询关键字**
* 软件清单：**yum list | grep 软件包名**
  > yum list 得配合 grep 来使用，yum list | grep 这个命令能显示软件的版本号，非常的实用。
* 查询某软件是否已经安装：**yum list installed | grep 软件包名**
    > 如果没有显示任何信息表示没有安装
* 缓存命令：
  ```shell
    # 清除缓存目录下的软件包
    yum clean package

    # 清除缓存目录下的 headers
    yum clean headers

    # 清除缓存目录下的旧的 headers
    yum clean oldheaders
  
    # 清除所有缓存
    yum clean all
    
    # 生成缓存
    yum makecache
    ```

* 不使用插件操作
```shell
  # 禁用插件更新
  yum update --noplugins
  
  # 禁用某个插件 
  # 使用--disableplugin= 这个选项指定，插件名之间用逗号分隔
  yum update --disableplugin=fastestmirror,refresh-pack*

```
* 仓库相关
```shell
  # 所有仓库列表
  yum repolist all

  # 已启用仓库列表
  yum repolist enabled

  # 禁用仓库列表
  yum repolist disabled

```


#### 换源

步骤：
1. 先装wget 
```shell
  yum install -y wget
```
2. 备份默认源配置文件
```shell
  mv /etc/yum.repos.d/CentOS-Base.rep /etc/yum.repos.d/CentOS-Base.repo.backup
```
> 要执行mv操作前，最好确认系统已经装了wget这下载软件  
> 因为**mv**操作是直接将CentOS-Base.rep这个文件进行更名，如果更了名后，就不能装wget了
> 如果安全为主，可以先使用复制**cp**操作进行备份，然后装wget，最后再删掉CentOS-Base.repo

3. 下载新的 CentOS-Base.repo 到 /etc/yum.repos.d/
```shell
  wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.163.com/.help/CentOS6-Base-163.repo
```
> 注意系统的版本要对应上

4. 清理缓存和生成新的缓存
```shell
  yum clean all
  yum makecache

```

#### 第三方源

* EPEL源:  
    EPEL 即**Extra Packages for Enterprise Linux**，提供了1W+的软件包。
* ~~RPMforge~~
> RPMForge 是被CentOS 社区认为是最安全也是最稳定的一个软件仓库。
> RPMForge 的源需要下载相应的rpm包来安装。
> RPMForge [官网](http://repoforge.org) 显示这个源很久没更，算是“废”了。
* RPMFusion：  
  这是为 Fedora和RHEL 提供额外RPM 软件包的第三方源。  
  这个源的安装是通过RPM 包来安装。  
  [中科大](http://mirrors.ustc.edu.cn/help/rpmfusion.html)、[清华](https://mirror.tuna.tsinghua.edu.cn/help/rpmfusion/) 等国内镜像都提供这个源RPM包。  
  具体安装请参考各RPM 包提供方的安装说明。  
  > 在 RHEL 或兼容发行版（如 CentOS ）上，您需要先启用 EPEL 源。--中科大 RPMFusion 安装备注。
* [Remi源](http://rpms.remirepo.net)：  
 这个源的包是**最新稳定版**，都是 Linux 骨灰级玩家编译，所以稳定性是有保证的。 



无论是安哪个第三方源，安装前先查看下系统已有的源
> ```shell
>   yum repolist
> ```

##### 安装 EPEL
```shell
  yum install epel-release
```

> 安装完第三方源，同样需要更新下缓存
> ```shell
>   yum makecache
> ```

##### 安装 Remi
```shell
  # 下载 Remi 对应版本的 rpm 包
  wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm
  # 安装
  rpm -Uvh remi-release-7.rpm
  # 删除包 (可选，你选择保留 rpm 包也行)
  rm -rf remi-release-7.rpm

  # 看下repo 配置文件中是否启用了 remi
  vim /etc/yum.repos.d/remi.repo
  # 其中有enabled=0 处,改为1，就能启用
  
  # 最后当然是贯例，清缓存，重新生成新缓存
  yum clean all
  yum makecache

```

#### yum 小工具

* yum-utils：  
  yum-utils 这个工具中，包含了一个非常实用的命令： **yum-config-manager**。

##### yum-config-manager 使用

```shell
  # 启用仓库：
  yum-config-manager --disable 仓库名

  # 禁用仓库：
  yum-config-manager --enable 仓库名
  
  # 添加一个源仓库
  yum-config-manager --add-repo 仓库地址

  # 示例
  # yum-config-manager --add-repo http://repos.fedorapeople.org/repos/dchen/apache-maven/epel-apache-maven.repo
  # yum-config-manager --enable epel-apache-maven

```

---

## <span id="cent_cs_info">常用软件安装和使用</span>

### <span id="cent_cs_ssh">启用 SSH 功能</span>

  检测是是否已经安装了openssh
  如果什么信息都不显示表示没有安装openssh
  yum list installed | grep openssh-server
  
  安装
  yum install openssh-server
  yum install openssh-clients
  > server 和 clients 都得装
  > 要想知道 openssh 是否装好，可以用以下命令测试：
  > ```shell
  >   ssh -V
  > ```
  > 如果没有出现 openssh 版本信息，就说明没有装 openssh-clients。

  配置ssh
  * 在 **/etc/ssh/** 目录下有**sshd_config**配置文件
  * 将端口号和访问 IP 地址的注释去掉
      ![centos_ssh_ip_port](./CentOS_Note.assets/centos_ssh_ip_port.png)
  * 允许 root 用户登录  
      ![centos_ssh_rootlogin](./CentOS_Note.assets/centos_ssh_rootlogin.png)
  * 将 **PermitTTY yes** 这一项的注释也去掉，并且将其值设置为 **yes**，才能开启远程登录
      ![centos_ssh_permittty](./CentOS_Note.assets/centos_ssh_permittty.png)
  * 将**PasswordAuthentication**这一项也注释去掉并设为**yes**，这是用户密码登录
      ![centos_ssh_pwauthentication](./CentOS_Note.assets/centos_ssh_pwauthentication.png)
	* 将 `UsePAM` 改为 `no`
  * 改 root 登录密码
  ```shell
   passwd
  ```
  > 输两次就行了

  *  开启 sshd 服务
  ```shell
    sudo service sshd start
  ```
   或重启 ssh 服务： 
  ```shell
  service ssh restart
  ```


  * 检测服务是否开启成功
  ```shell
    ps -e | grep sshd
  ```


