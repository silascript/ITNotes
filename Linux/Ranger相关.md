# Ranger相关





## 安装及基础配置



```shell
sudo pacman -S ranger

sudo ln -s /usr/bin/ranger /usr/bin/ra
# 在~/.config/ranger目录下生成一堆配置文件。主要配的是scope.sh和rc.conf
ranger --copy-config=all
# 安装图标
git clone https://github.com/alexanderjeurissen/ranger_devicons ~/.config/ranger/plugins/ranger_devicons
echo export RANGER_LOAD_DEFAULT_RC=FALSE >> ~/.bashrc
```





## 预览



### 图片预览

使用**w3m**或**ueberzug**



w3m是终端web浏览器

如果终端使用w3m不支持不生效，就使用**ueberzug**

配置rc.conf如下:

```shell
set preview_images true
set preview_images_method ueberzug
```