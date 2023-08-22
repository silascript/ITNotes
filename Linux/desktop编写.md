

在`/usr/share/applications/`目录中新建一个后缀名为`desktop`的文件

```desktop
[Desktop Entry] (这里大小写敏感，写错一个就不会显示)
Type=Application 类型
Name=名称
Name[zh_CN]=简中名称
Name[zh_TW]=繁中名称
Comment=鼠标放在图标上时的提示文字
Comment[zh_CN]=与上类似不过是简体中文
Comment[zh_TW]=繁中的提示文字
Icon=图标路径
Exec=可执行文件的路径
Categories=程序的分类标签，多个标签使用;号分隔(Application;IDE;Development;Editor)
Terminal=false 是否使用启用终端
StartupNotify=true
```

> [!example] 示例
> 
> ```desktop
> [Desktop Entry]
> Name=Obsidian
> Exec=/opt/appimages/Obsidian.AppImage --no-sandbox %U
> Terminal=false
> Type=Application
> Icon=obsidian
> Categories=Utility;
> 
> ```



刷新桌面图标：

刷用户目录下的：

```shell
update-desktop-database ~/.local/share/applications
```

刷是根下的：

```shell
sudo update-desktop-database /usr/share/applications
```

[arch 相关wiki](https://wiki.archlinuxcn.org/wiki/%E6%A1%8C%E9%9D%A2%E9%A1%B9)










