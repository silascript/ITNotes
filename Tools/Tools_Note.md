
# 常用小工具笔记

---

## 目录
* [视频下载工具](#tools_vdowners)
* [重命名工具](#tools_rename)

---

## <span id="tools_vdowners">视频下载工具</span>

常用的视频下载工具有：
* [lux](#tools_vdowners_lux)


---

### <span id="tools_vdowners_lux">lux</span>

[lux](https://github.com/iawia002/lux) 原名叫「annie」，是一款命令行视频下载工具，用来下载 B站等视频网站的视频是挺爽的。

---

#### 安装

Window 下推荐使用 [Scoop](https://github.com/ScoopInstaller/scoop) 来安装。Linux 下就使用各发行版的包管理器来装了，非常简单。

---

#### lux 常用命令

##### 查询视频信息

```shell
lux -i 网址
```
这条命令会出现诸如该视频的信息，如有多少种分辨率可下载，每种分辨率的体积大小。


##### 下载单个视频

下载视频，需要指定要下载的分辨率，`-f` 这个参数的值是分辨率的标识符，可以通过上面的 `lux -i` 命令查询出来。

示例：
```shell
lux -f 64-12 "https://www.bilibili.com/video/BV18p4y167Md?p=1"
```


##### 批量下载

下载专辑所有视频。
示例：
```shell
lux -p -f 64-12 https://github.com/iawia002/lux
```

如果专辑中的「分p」太多，防止下载中途出问题，一般习惯将其分多次下载，那每次就可以指定下载连续的多个视频。所以在 `-p` 参数后再增加 `--start` 和 `--end` 两个选项用来指定列表分段起始及结束点。
示例：
```shell
lux -p --start 1 --end 10 -f 64-12 https://www.bilibili.com/video/BV18p4y167Md
```

如果下载多个视频不是连续的，可以使用 `--items` 指定要下载哪几个视频，这个选项值为「分p」的数字值，各数字间使用英文的「,」分隔。
示例：
```shell
lux -f 64-12 -p --items 91,93,95 https://www.bilibili.com/video/BV18p4y167Md
```

---

##### 从外部文件列表下载

可以自定义个下载列表文件，在 lux 中指定下载。

示例：
```shell
lux -F /path/to/links.txt
```

---


## <span id="tools_rename">批量重命名</span>


---

### <span id="tools_rename_f2">F2</span>


[f2](https://github.com/ayoisaiah/f2) 是一款在命令行下可以批量重命名的小工具。


#### <span id="tools_rename_f2_install">F2 安装</span>

F2 可以通过 npm 进行安装：
```shell
npm i @ayoisaiah/f2 -g
```

也可以通过各 Linux 发行版中的包管理器进行安装

Arch Linux：
```shell
yay -S f2-bin
```

Windows 下推荐使用 [Scoop](https://github.com/ScoopInstaller/scoop) 进行安装：
```shell
scoop bucket add ayoisaiah-scoop-bucket https://github.com/ayoisaiah/scoop-bucket
scoop install f2
```


更多安装方式请参考 [f2 wiki](https://github.com/ayoisaiah/f2/wiki/Installation)。


---


#### <span id="tools_rename_f2_commands">F2 常用命令</span>


`f2` 这是 f2 的主命令
`-f` 查找要修改的部分
`-r` 替换成什么
`-x` 执行重命名操作

示例：
```shell
f2 -f '' -r ''
```
如果不加 `-x`，那就不会执行重命名操作，只是显示预览效果。



删除原名称中部分字符，如下示例中，要删除 **test ** 这几个字符：
```shell
f2 -f 'test ' -r ''
┌───────────────────────────────┐
| ORIGINAL   | RENAMED | STATUS |
| ***************************** |
| test 1.txt | 1.TXT   | ok     |
| test 2.txt | 2.TXT   | ok     |
| test 3.txt | 3.TXT   | ok     |
└───────────────────────────────┘
```

f2 内置了一些变量，能够快速实现常用功能：
`{{tr.up}}` 将字符转换成大写
`{{tr.lw}}` 将字符转换成小写



更多的 f2 的使用说明，请参考[f2 wiki](https://github.com/ayoisaiah/f2/wiki) 文档。


---

