# Linux 下安装配置Apache

## 预置组件

官方要求，必须安装**APR**、**APR-Util**、**APR-iconv**、**PCRE**、gcc

源码安装3个步骤: **配置**(configure)、**编译**(make)、**安装**(make install)。

**配置**

```shell
./configure --prefix
```

各组件源码安装示例:

```shell


# 这三个安装顺序是有点讲究的
# apr-iconv要指定apr位置，所以apr得先装
# apr-util得指定apr和apr-iconv的位置，所以apr-util最后才装
# 当然这三个组件安装前像什么gcc得装好了，这个可能通过如ubuntu的apt，archlinux的pacman,红帽系的yum等软件管理器安装

#apr
./configure --prefix=/usr/local/apr
make && make install
  
#apr-iconv
./configure --prefix=/usr/local/apr-iconv --with-apr=/usr/local/apr
make && make install
  
# apr-util
./configure --prefix=/usr/local/apr-util --with-apr=/usr/local/apr --with-apr-iconv=/usr/local/apr-iconv/bin/apriconv
make && make install


```



安装apache的依赖apr时报rm: cannot remove ‘libtoolT’: No such file or directory的错



##### 解决方法

###### 	第一种方法：

​	1. 确认libtool是否已经安装，如果没有安装的话，则先安装libtool

​	2. 分别执行以下三条命令：

```shell
autoreconf --force --install
libtoolize --automake --force
automake --force --add-missing
```





###### 	第二种方法：

​	编辑configure
​		找到RM&quot;RM &quot;RM"cfgfile" ，注释掉 $RM "$cfgfile" ，或者直接删除也可以， 然后重新运行./configure即可。

```shell
$RM "$cfgfile" #将其注释即可
```





