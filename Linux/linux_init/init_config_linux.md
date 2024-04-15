
# Manjaro Linux 安装初始配置

---


## 中国源


* [南京大学](https://mirror.nju.edu.cn)
* [北大](https://mirrors.pku.edu.cn)
* [中科大](https://mirrors.ustc.edu.cn)
* [清华](https://mirrors.tuna.tsinghua.edu.cn)
* [上海交大](https://mirror.sjtu.edu.cn)
* [吉林大学](https://mirrors.jlu.edu.cn)
* [浙大](https://mirrors.zju.edu.cn)
* [南阳理工](https://mirror.nyist.edu.cn)


北大、南阳理工就不要选了，速度真不行。优先选择清华和中科大，这俩最靠谱。


------------------------------

测试最快的中国源：

`sudo pacman-mirrors -i -c China -m rank`

--------------------------


### archlinuxcn

archlinuxcn这个源有中国特有的一个包。

在 `/etc/pacman.conf` 文件末尾添加以下两行：

```conf
[archlinuxcn]
Server = https://mirrors.pku.edu.cn/archlinuxcn/$arch
```

以添加并启用 Arch Linux CN 镜像源

之后用 -Sy 安装 `archlinuxcn-keyring` 包导入 GPG key：

`sudo pacman -Sy archlinuxcn-keyring`

最后执行命令：`sudo pacman -Syyu`，强制刷新软件包列表。



----

## 修改 Grub

grub模板：`/etc/default/grub`

每当修改 `/etc/default/grub` 或者 `/etc/grub.d/` 中的文件之后，都需要再次生成主配置文件。

制成配置文件：`grub-mkconfig -o /boot/grub/grub.cfg`

重启电脑看效果。

---







