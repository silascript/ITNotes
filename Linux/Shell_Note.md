---
aliases:
  - 
tags:
  - shell
  - linux
  - list
created: 2023-08-18 19:44:52
modified: 2023-08-28 22:12:46
---
# Shell 笔记

---
## 目录

---

## <span id="shell_path">路径</span>

```shell
# 显示当前路径
echo $PWD
```
> [!tip] 
> 
> `$PWD`，跟 echo 命令一起使用时，必须大写。

## <span id="shell_array">数组</span>

取最后一个元素：`arr[-1]`。
> [!tip] 老语法
> 
> `${#arr[@]-1}`：`${#arr[@]}` 这是取数组长度，那数组长度 -1 就得到数组最后一个元素的索引值。

---

## <span id="shell_string">字符串</span>

### 截取

```shell

# # 是从左向右，一个#是取第一个，##是取最后一个
# % 是从右向左 一个%是取第一个，%%是取最后一个
## 
s1="https://github.com/rexdf/ChineseLocalization.git"

# 从左向右取第一个.后的字符串
# 即取到的是 com/rexdf/ChineseLocalization.git
echo "${s1#*.}"

# 从左向右取最后一个.后的字符串
# 即取到的是 git
echo "${s1##*.}"

# 从右向左取.后的第一个字符串
# 即取到的是 https://github.com/rexdf/ChineseLocalization
echo "${s1%.*}"

# 从右向左取最后一个.后的字符串
# 取到的是 http://github
echo "${s1%%.*}"

```

---

## 视频清单

* [2023年最通俗易懂的Shell教程](https://www.bilibili.com/video/BV1Tu411W7Up)
* [2023年最新Shell自动化开发全套顶级天花板教程](https://www.bilibili.com/video/BV1494y1i7ca)

