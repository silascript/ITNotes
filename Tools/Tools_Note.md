---
aliases: []
tags: []
created: 2023-08-18 19:44:52
modified: 2024-02-20 06:04:56
---

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

[lux](https://github.com/iawia002/lux) 原名叫「annie」，是一款命令行视频下载工具，用来下载 B 站等视频网站的视频是挺爽的。

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

```shell
lux -p --items 29 -c ~/mysoft/Browsers/cookies.txt -i 网址
```

> [!info] 查看视频集合某 p 的信息
> 
> 使用 `-p --items p序号` 指定具体 p
> 
> `-c` 是 [[指定 Cookies](#指定%20Cookies)]，因为常常因为权限问题会只能查看到**480p**，实际视频可能是**1080**的，如要查看视频完整信息，就得使用到 Cookies。

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

如果专辑中的「分 p」太多，防止下载中途出问题，一般习惯将其分多次下载，那每次就可以指定下载连续的多个视频。所以在 `-p` 参数后再增加 `--start` 和 `--end` 两个选项用来指定列表分段起始及结束点。
示例：
```shell
lux -p --start 1 --end 10 -f 64-12 https://www.bilibili.com/video/BV18p4y167Md
```

如果下载多个视频不是连续的，可以使用 `--items` 指定要下载哪几个视频，这个选项值为「分 p」的数字值，各数字间使用英文的「,」分隔。
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

##### 指定 Cookies

有些网站下载视频，特别是下载 720 或 1080 等高清视频时，可以需要账号已登录，如「B 站」，这时就需要从 Cookie 中读取已登录账号的状态的。所以得在指定相应的 Cookies 文件。

> [!example] 语法格式
>  
> `lux -i -c ~/mysoft/NetBrower/edge_cookies.txt "xxxx"`
> 
> `-c` 后就是 Cookies 文件路径

> [!example] 示例
> `lux -p -c ~/mysoft/NetBrower/edge_cookies.txt -f 80-7 https://www.bilibili.com/video/xxxxx`

###### 如果导出 Cookies

使用 [EditThisCookie](../Browsers/Chrome_Note.md#EditThisCookie) 这个插件就能快速「导出」Cookies 文件。

> [!tip] Cookies 文件内容格式
> lux 指定的 cookies 文件的后缀名可以是任意，但内容必须是 「Netscape」的，所以得在设置里设置导出的格式为「Netscape HTTP Cookie」。
> 
> 相关链接：
> 
> [请求指导cookie.txt详细的使用方法 · Issue #739](https://github.com/iawia002/lux/issues/739)

使用 [Cookie Editor Plus ](https://microsoftedge.microsoft.com/addons/detail/cookie-editor-plus/nbmajjcfigmlcnikhnfhhicidleefhpp?hl=zh-CN) 也能达到相同效果。

### 常用命令示例

```shell

# 查看视频信息
lux -i xxx

# 使用指定Cookie，查看视频信息
# 某些视频需要Cookie才能查看视频信息
# 如B站，如果使用上面的命令，得到的视频信息就只显示最高480P画质，这是由于B站对未登录账号观看视频画质的限制
lux -i -c ~/mysoft/Browsers/cookies.txt xxx


# 下载1080P单个视频
lux -f 80-7 xxx

# 使用指定Cookie 下载单个视频 分辨率为1080P
# 与查看视频信息一样，某些网站得登录才能下载更高画质的视频，所以就需要使用到Cookie
lux -c ~/mysoft/Browsers/cookies.txt -f 80-7 xxxx

# 使用指定Cookie 下载合集所有视频 分辨率为1080P
lux -p -c ~/mysoft/Browsers/cookies.txt -f 80-7 xxx

# 使用指定Cookie 下载合集中 47到50的视频 分辨率为1080P
lux -p --start 47 --end 50 -c ~/mysoft/Browsers/cookies.txt -f 80-7 xxx

# 使用指定Cookie 下载合集中 29和30两个视频 分辨率为1080P
lux -p --items 29,30 -c ~/mysoft/Browsers/cookies.txt -f 80-7 xxx

```

#### 常见分辨率

所谓「分辨率」，就是 `-f` 参数所要指定的「标识」，即你想要下载到哪种画质的视频。

```shell

 [80-7]  -------------------
 Quality:         高清 1080P avc1.640032
 # download with: lux -f 80-7 ...

 [64-7]  -------------------
 Quality:         高清 720P avc1.640028
 # download with: lux -f 64-7 ...

 [32-7]  -------------------
 Quality:         清晰 480P avc1.64001F
 # download with: lux -f 32-7 ...

 [16-7]  -------------------
 Quality:         流畅 360P avc1.64001E
 # download with: lux -f 16-7

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

更多的 f2 的使用说明，请参考 [f2 wiki](https://github.com/ayoisaiah/f2/wiki) 文档。

---

## Google 相关

### Google 镜像

* Google 搜索：https://so.niostack.com/
* Google 搜索：https://www.sowai.cn/
* ~~Google 搜索：https://xn--flw351e.ml/~~
* ~~Google 搜索：https://search.njau.cf/~~
* ~~Google 搜索：https://search.ahnu.cf/~~
* ~~Google 搜索：https://search.fuyeor.com/zh-cn/Google~~
* Google 学术：https://ac.scmor.com/
* Google 学术：https://scholar.lanfanshu.cn/

### 其他相关

* [谷粉搜导航](https://gufenso.coderschool.cc/)

> [!important] 安全提醒
> 
> **不要在镜像站上登录谷歌账户**！

#### 参考资料

* [2023-2024 最新可用Google镜像地址 长期更新 - 哔哩哔哩](https://www.bilibili.com/opus/886060167271022609)
* [谷歌镜像，谷歌学术镜像——2024年1月最新 - 知乎](https://zhuanlan.zhihu.com/p/607795300?utm_id=0&wd=&eqid=a982987d007d0be9000000056583f3f6)
