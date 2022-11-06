# Python笔记
---

## 安装和常用

### 更新

```python
# 列出可以更新的包
pip list --outdated

# 更新指定的包
pip install --upgrade xxx

# 更新pip
pip install --upgrade pip
# 或 使用python -m 来更新pip
python -m pip install --upgrade pip


```

### pip 换源

#### 临时换源并安装指定包
```python
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 包名
```

清华的镜像源每五分钟更新一次，大而全，推荐大家使用。
国内其他源:
* 清华：https://pypi.tuna.tsinghua.edu.cn/simple
* 阿里云：http://mirrors.aliyun.com/pypi/simple/
* 中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/
* 华中理工大学：http://pypi.hustunique.com/
* 山东理工大学：http://pypi.sdutlinux.org/ 
* 豆瓣：http://pypi.douban.com/simple/


#### 永久性换源

1. 第一种方式：
```python

# 清华源
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
# 阿里源
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
# 腾讯源
pip config set global.index-url http://mirrors.cloud.tencent.com/pypi/simple
# 豆瓣源
pip config set global.index-url http://pypi.douban.com/simple/

```
2. 第二种方式：

直接修改pip配置文件
pip的配置文件是放在.pip目录下的pip.conf文件中(windows是pip.ini文件)
示例：
```config
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

---

## <span id="python_conda">conda</span>


### conda 换源

#### 生成conda配置文件

以清华源为例：
```shell
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes # 生成".condarc"文件
```

在生成的 `.condarc` 文件中追加以下配置：
```
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloudtext
```





