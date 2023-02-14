---
aliases:
  - 
tags:
  - editor
  - vscode
created: 2023-01-13, 12:27:45
modified: 2023-01-30, 7:50:59
---
# VSCode 笔记

---

## 目录

* [插件](#插件)
* [快捷键](#快捷键)

---

## 插件

[常用插件](Editors_Note.md#VSCode#常用插件)

---

## 快捷键

* `Ctrl+Shift+p`：打开命令面板
* `Ctrl+p`：查找并打开文件
* `Ctrl+Alt+-`：Go Back 跳回之前的文件
* `Ctrl+Shift+t`：恢复刚关闭的文件

官方给的快捷键帮助文档：

* [Windows 版本](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-windows.pdf)
* [Linux](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-linux.pdf)

---

## <span id="vscode_settings">配置</span>

### <span id="vscode_settings_profile">code-profile</span>

「code-profile」是 [VSCode](https://code.visualstudio.com/) 1.75 版本开始提供的非常好用的新功能。

Profile 新特性，能够使在不同「工作流」（场景）使用不同的配置，我们就能根据当前实际需求，启用或安装不同插件，这样就解决了 VSCode 插件装得过多，占用资源大的缺点了。

Profile 新功能还支持将配置「导入」和「导出」，非常适合将各个配置单独导出到本地保存，或将本地保存的配置文件导入到 VSCode 中。而且导出和导入功能还支持向 [github](https://github.com)。

### <span id="vscode__synchronize">同步</span>

配置完了，自然需要将配置同步到「云」上。当然可以手动导出配置然后上传到网盘上，下次再导入，但这操作有点麻烦。

所以建议使用同步方式将配置同步保存到网络上。

而同步又有两大类：
1. 使用同步插件将配置同步到 [gist](https://gist.github.com/)
	此种方式，用到的插件 [VSCode-Syncing](Editors_Note.md#editors_vscode_extensions_syncing)。

2. 使用 VSCode 内置的「同步」功能。
	使用这功能时，登录时，最好就不要用 github 登了，麻烦太多。用微软 outlook 方式，当然没有 outlook，微软也非常贴心，可以让你使用 githut 帐号授权给 outlook，反正 github 都是微软的，「左口袋到右口袋」，没有区别。
	
	试用过这「同步」功能后，很爽，它除了拥有与插件 [VSCode-Syncing](Editors_Note.md#editors_vscode_extensions_syncing) 一样的，将原有 settings 备份，还能将所有你设置的 [Profile](#code-profile) 设置也备份了，这样下次换设备什么，就能从「云」上将备份的 Profile 设置同步下来，真是非常方便！
	从某种意义内置的「同步」功能已经可以替换 [VSCode-Syncing](Editors_Note.md#editors_vscode_extensions_syncing) 插件了。

	网上网友的分享，也证明官方的同步方案是挺「香」，但还是存在一个问题：不同操作系统的差异性，官方同步不能像 [VSCode-Syncing](Editors_Note.md#editors_vscode_extensions_syncing) 插件那样，不同平台下各自设不同的 [gist](https://gist.github.com/) 来适配相应的平台。但无论如何，整体而已，官方内置的「同步」功能确实非常优秀。

> [!tip] 关于同步的各种网络资料
> * [VSCode同步扩展插件的3种方法](https://segmentfault.com/a/1190000039769766)
> * [VSCode官方的配置同步方案](https://juejin.cn/post/7066622158184644621)

---

