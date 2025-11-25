---
aliases: []
tags:
  - shell
  - linux
  - example
created: 2023-09-23 01:34:32
modified: 2025-11-24 12:42:06
---

# Shell 脚本示例笔记

---

## 说明

这个笔记主要记录些常用的 Shell 脚本代码版本，当成示例库。

---

## 示例

### 遍历

* 示例 1
```shell
# 遍历 data 目录下txt文件并备份，备份文件名称加上年月日为后缀
# 1.txt -> 1.txt_20230923
suffix=`date +%Y%m%d`
   
for f_temp in `find data/ -type f -name "*.txt"`
do
   echo "备份文件$f_temp"
 cp ${f_temp} ${f_temp}_${suffix}                                                                                 
done
```

* 示例 2
```shell
local exuid_arr=()

# 过滤掉空行及使用#注释的行
for line in $(cat $exlist_path | grep -v ^$ | grep -v ^\#); do
	# 把每行扩展的 uid 存储进数组中
	exuid_arr+=($line)
done

```

### 判断参数个数

```shell
if [[ $# -eq 0 ]]; then
	echo -e "\e[93m必须输入一个要查询的字符串! \n \e[0m"
	return
fi
```

### 路径相关


示例1：确保目录路径不以斜杠结尾

```shell
SRC_DIR=${SRC_DIR%/}
```

---

## 相关笔记

* [Shell 笔记](Shell_Note.md)
* [Shell 资料清单](Shell_Material.md)
* [Shell 视频清单](Shell_Videos.md)
* [Linux 笔记](../Linux_Note.md)

