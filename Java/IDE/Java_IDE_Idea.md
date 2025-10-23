---
aliases: []
tags:
  - java
  - ide
  - idea
created: 2025-10-21 03:20:34
modified: 2025-10-22 20:14:27
---

# Java Idea 笔记

---

## 安装

### Linux

在 `.local/share/applications` 目录新建一个 `desktop` 文件，格式如下：

```desktop
[Desktop Entry]
Version=1.0
Type=Application
Name=IntelliJ Idea 2024
Icon=idea.png
Exec=/opt/JavaIDE/idea_iu/bin/idea
Comment=The smartest JavaScript IDE
Categories=Development;IDE;
Terminal=false
StartupWMClass=jetbrains-idea
```

* `Icon`：图标
* `Exec`：可执行文件
* `StartupWMClass`：确保 `desktop` 文件启动程序时，能在任务栏、快捷键等地方正确显示程序的图标和功能的。
> [!tip] 
> 
> `StartupWMClass` 获取请参考：[Linux_Note#StartupWMClass](../../Linux/Linux_Note.md#StartupWMClass)

#### crack

到 [https://blog.idejihuo.com](https://blog.idejihuo.com) 找相应的激活工具，按照其说明激活应该就可以了。

##### 激活流程

> [!tip] 
> 
> 可能不同版本有点小差异，但核心流程是基本一致的。

###### 清理旧版本

可以 `cat` 下 `.profile` 配置，看有没有以下类似的环境变量配置：

`___MY_VMOPTIONS_SHELL_FILE="${HOME}/.jetbrains.vmoptions.sh"; if [ -f "${___MY_VMOPTIONS_SHELL_FILE}" ]; then . "${___MY_VMOPTIONS_SHELL_FILE}"; fi`

这个配置是配置 `.jetbrains.vmoptions.sh` 脚本的。

`.profile` 被配置后，要么 `source` 下 `.proifle`，要么重启下电脑，让 `.profile` 生效 -- 建议如果没有问题，还是重启下电脑，因为有时 `source` 不一定会让 `profile` 生效。

###### 生成 Idea 配置目录

> [!info] 
> 
> 安装完 idea 的 [Linux](../../Linux/Linux_Note.md) 版其实就是把 idea 解个包而已，run 下 `bin` 下的 `idea`，启动 idea。
> 
> 这时用 `ls -al` 命令看下 `~/.config/` 目录下是否生成 `JetBrains/IntelliJIdea2024.3`,这两层目录。这就是 Idea 的配置目录。

3. 如果生成了，关了 idea。到激活工具的目录中，run 下 对应操作系统的激活脚本（可能得赋予相应的权限，如 `755`：`chmod 755 jetbra-free-linux-*`）。现在新版本激活工具，什么启动一个 `http://127.0.0.1:8123` 页面，可以自行随便填写激活信息，可以将激活有效时间设到 2099 年，然后 `submit`。在页面中，需要激活哪个 Idea 软件就点击哪个，出现 `cracked` 并生成激活码，Idea 就应该激活成功了！

> [!info] 
> 
> 老的激活工具，会在用户目录下生成 `.jetbrains.vmoptions.sh` 这个脚本，并且在 `.profile` 配置文件中添加加载 `.jetbrains.vmoptions.sh` 脚本相关配置：
>
>```shell
>#!/bin/sh
>export IDEA_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/idea.vmoptions"
>export CLION_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/clion.vmoptions"
>export PHPSTORM_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/phpstorm.vmoptions"
>export GOLAND_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/goland.vmoptions"
>export PYCHARM_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/pycharm.vmoptions"
>export WEBSTORM_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/webstorm.vmoptions"
>export WEBIDE_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/webide.vmoptions"
>export RIDER_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/rider.vmoptions"
>export DATAGRIP_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/datagrip.vmoptions"
>export RUBYMINE_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/rubymine.vmoptions"
>export DATASPELL_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/dataspell.vmoptions"
>export AQUA_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/aqua.vmoptions"
>export RUSTROVER_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/rustrover.vmoptions"
>export GATEWAY_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/gateway.vmoptions"
>export JETBRAINS_CLIENT_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/jetbrains_client.vmoptions"
>export JETBRAINSCLIENT_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/jetbrainsclient.vmoptions"
>export STUDIO_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/studio.vmoptions"
>export DEVECOSTUDIO_VM_OPTIONS="/home/silascript/mysoft/Java/IDE/IDEA/jihuo-tool-2024.3.1/jetbra/vmoptions/devecostudio.vmoptions"
> ```
>

###### vmoptions 配置

所谓激活，其实就是在启动时在 Idea 的配置文件 `vmoptions` 文件中配置激活工具。

> [!info] 
> 
> 老版本是 生成 `.jetbrains.vmoptions.sh` 脚本，通过这个脚本「间接」地配置了「激活工具」的相关的环境变量。

新的版本是直接在 `.config/JetBrains/IntelliJIdeaxxxx` 目录下的 `vmoptions` 中配置：

```properties
--add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED
--add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED
-javaagent:/home/silascript/mysoft/Java/IDE/IDEA/jetbra-free-2099/.jetbra-free/static/ja-netfilter/ja-netfilter.jar=jetbrains
```

> [!important] 
> 
> 最后那个 `-javaagent` 属性配的就是激活工具的路径。
> 
> 注意！可能默认是没有 `idea64.vmoptions` 这个文件的，而只有 `idea.vmoptions` 文件。如果是这样，有可能会激活失效，因为现在的 Idea 都是 64 位的。
> 
> 解决方案：把 `idea.vmoptions` 复制一份更名为 `idea64.vmoptions`，重启 IDE 或电脑后，启动激活工具再走一遍激活流程就应该能激活成功了！

---

## 配置

### 优化

`idea.vmoptions` 或 `idea64.vmoptions` 文件中添加 `-Xmx2048m`，即将堆内存上限限制在 2G--`2048` 这个数值可以根据自已需求自行设置。

> [!info] 
> 
> 当然这个设置也可以通过 Idea 的 `帮助`-->`更改内存设置` 中设置。
> 
> 也可以通过点击 `帮助`-->`编辑自定义虚拟机/VM选项`，在 Idea 中直接打开 `idea.vmoptions` 或 `idea64.vmoptions`，直接编辑。

* `-Xms`：堆内存最小值
* `-Xmx`：堆内存最大值

---

## 快捷键

### 常用

* `Ctrl+Alt+l`：格式化

---

## 插件

### vim 相关

#### IdeaVim

Idea 上实现 vim 插件的插件。

这个插件的配置文件是 `~/.ideavimrc` 这个文件。

#### IdeaVim-EasyMotion

[IdeaVim-EasyMotion](https://github.com/AlexPl292/IdeaVim-EasyMotion) 是在 Idea 上实现 [easymotion](../../vim/Vim_Plugin.md#easymotion) 插件效果的插件。

这个插件用的配置文件是与 [IdeaVim](#IdeaVim) 一样的，即 `~/.ideavimrc`。

##### 启用 easymotion

默认就算装了这个插件，它还是没有启用的。

要启用 IdeaVim-EasyMotion 这插件，就在 `~/.ideavimrc` 中添加 `set easymotion` 配置。

---

## 相关笔记

* [Java 笔记](../Java_Note.md)
* [Java 资料清单](../Java_Material.md)