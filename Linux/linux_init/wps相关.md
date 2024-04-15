---
aliases: []
tags:
  - archlinux
  - manjaro
  - wps
created: 2024-04-15 11:29:21
modified: 2024-04-15 11:32:23
---

## 安装 WPS

`yay -S wps-office-cn wps-office-mui-zh-cn ttf-wps-fonts`

> [!info]
> 只装 wps-office-cn，好像还是英文的。
>
> 装上 wp-office-mui-zh-cn 才有了中文界面。
>
> ttf-wps-fonts 字体了。

如果安装出错，很有可能是缺了 `base-devel`，manjaro 竟然不自带这货！

安装下就好了：`yay -S base-devel`

---

## 禁止 WPS for linux 后台偷联网

运行：`ps -ef|grep wpscloudsvr`

定位 `wpscloudsvr` 路径：
有可能是 `/usr/lib/office6/wpscloudsvr`，也有可能是别的。

杀掉进程：`killall wpscloudsvr`

禁掉该程序所有权限：`sudo chmod 000 /usr/lib/office6/wpscloudsvr `

---

## PDF 打不开

出现问题的原因是，WPS PDF 依赖于 libtiff5，而 Arch 由于滚动更新的特性，已经升级到了 libtiff6，因此 WPS PDF 无法正常工作。同时 aur 中打包的 WPS 也没有将 libtiff5 作为依赖，所以会报错。

只要把 `libtiff5` 这个库安装上就能打开了。

`yay -S libtiff5`

> [!tip] 相关资料
>
> * [pdf打不开](https://bbs.wps.cn/topic/5586)
> * [shellraining wpspdf](https://shellraining.github.io/tools/wpsPDF)

---

