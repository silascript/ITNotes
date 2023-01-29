---
aliases: 
tags: chrome browser google plugin
created: 2023-01-29, 6:47:52
modified: 2023-01-29, 8:34:57
---

# Chrome 浏览器笔记

---



## <span id="chrome_plugins">插件</span>


### <span id="chrome_plugins_issue">插件相关问题</span>


#### <span id="chrome_plugins_issue_relocation">插件迁移</span>

Chrome 的插件能用在 Chrome 系浏览器上，但如果反过来，[Edge] 中有一些好用的插件，而这些插件是 Edge 「特供」的，想要迁移到 Chrome 或 Chrome 系其他浏览器上，又如何实现？

这是件有趣的事情。因为最近发现 Edge 上有多款「梯子」软件非常好用，而 Edge 是有自己的扩展库的，所以国人是能装上插件，而如果使用原版 Chrome 或 Chrome 系其他没有本地化插件库的浏览器时，安装插件就变得非常麻烦的事情。其他常用插件还能通过国内各大 Chrome 插件网站进行安装，虽然版本有可能旧点，但毕竟还是能装上用的，但像「梯子」插件这种极度敏感的插件，那些插件网站根本不敢摆出来，而 Edge 的扩展库偏偏就有几款，而且还是能用的那种，虽然碰到「特殊节日」一样被「封印」，但大多数情况还是可能使用的，那这些好用的「梯子」只能「困」在 Edge 这池子里，显然是「屈才」了，我就想将它们弄到 Chrome 及 Chrome 系其他浏览器上，所以就研究了一下，大概总结出一个大致的迁移方法。


##### 获取插件目录路径

首先，要「迁移」插件，得先知道它们被存在在哪。这就涉及到插件存在目录。

而 Chrome 系浏览器的插件存放目录是在「用户目录下」，那进一步就得知道「用户目录」的路径。

在地址栏中输入：`chrome://version/`，显示出当前 Chrome 浏览器的版本信息。

在这堆信息中，其中 **个人资料路径** 中的路径是我们需要的。
如：`F:\scoop\locals\apps\googlechrome\current\User Data\Default` 

> [!tip]
> 这个就是我的 Chrome 浏览器的「用户目录」，用户数据都保存在这个目录中。
> 当然例子中的的路径，不是一般情况会出现的，一般用户的 Chrome 用户目录应该是这样的：
>
> `C:\User名\AppData\Local\Chrome\User Data\Default`
>
>  又如 Edge： `C:\Users\xxx\AppData\Local\Microsoft\Edge\User Data\Default`
> 
> 另外，在 Linux 系统中，用户目录是放在 `.config` 目录中

> [!tip] Linux 用户目录路径示例
> `~/.config/chromium/Default`
> 
> `~/.config/microsoft-edge/Default`
> 
> `~/.config/vivaldi/Default`

而插件或扩展数据就存放在用户数据目录下的 `Extensions` 子目录中。

> [!example] 查看 Windows 下 Chrome 插件目录
> ```
> # silascript @ PC-202107291101 in ~: [07:20:44]
>$ ls 'F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions'
> Directory: F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions
>Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>d----           2023/1/29     6:39                ncennffkjdiamlpmcbajkmaiiiddgioo
>d----           2023/1/29     6:39                ngpampappnmepgilojfohadhhmbhlaek
> 
> ```
>

上面的例子中，插件目录存放有两个插件。最后那一长串东西是这个插件的 **ID**，这是这个插件的唯一标识，同时它也是这个插件的数据目录名，而这个插件目录下，是这插件不同版本的目录，如下面这下例子：

> [! example] 查看插件目录
> ```
> ls 'F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions\ncennffkjdiamlpmcbajkmaiiiddgioo\'
>    Directory: F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions\ncennffkjdiamlpmcbajkmaiiiddgioo
> Mode                 LastWriteTime         Length Name
>----                 -------------         ------ ----
>d----           2023/1/29     6:39                3.32_0
> 
>```
> 

这个插件里只有一个 3.32 版本。

知道了插件目录结构后，要想插件迁移并安装成功，还要知道两个事情：
1. 要「迁移」某插件时，得把那个插件 ID 为目录名的目录拷到目录浏览器 `Extensions` 目录下
2. 在「目标浏览器」安装目录时，指定的插件得指定到版本号那个目录


##### 查看插件 ID

通过上面插件目录结构的查询，可以知道，要迁移插件，得知道插件具体的 ID。

1. 到扩展程序页面，点击相要迁移的插件的「详情」或「详细信息」
2. 正常情况，这个插件的详细信息是不包括该插件的 ID，要想看到该插件 ID，就得打开「开发者模式」

##### 迁移及安装插件

上面得到了插件的 ID，只要把该插件 ID 拼接到 `Extensions` 路径后，就得到了该插件的路径，这样就能将这插件复制到目标浏览器的 `Extensions` 目录下。

> [!example] 复制插件
> ```shell
> cp 'C:\Users\silascript\AppData\Local\Microsoft\Edge\User Data\Default\Extensions\lpmhmbogdfoddanepgnbngfeofdjdnge\' -Recurse 'F:\scoop\locals\apps\googlechrome\current\User Data\Default\Extensions\' -Force
>```
> 

吐槽下，PowerShell 真难用，命令恶心了！


###### 安装插件

复制成功后，就到了安装插件的环节了。要想安装 `Extensions` 目录中的插件，也得打开 `开发者模式`，这时，会看到扩展页面多出一个「加载已解压的扩展程序」的按钮，点击这按钮，进入目录插件的目录中，如果复制成功，应该看到还有版本号作目录名的子目录，再选择一个版本目录 -- 自然选最新的，最后点击「选择文件夹」按钮确定，正常情况就会加载进扩展页面，这就安装成功了！





---
