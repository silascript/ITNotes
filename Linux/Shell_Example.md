---
aliases: []
tags:
  - shell
  - linux
  - example
created: 2023-09-23 01:34:32
modified: 2024-03-03 22:48:34
---
# Shell 脚本示例笔记

---

## 说明

这个笔记主要记录些常用的 Shell 脚本代码版本，当成示例库。

---

## 示例

### 遍历

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

### 判断参数个数

```shell
if [[ $# -eq 0 ]]; then
	echo -e "\e[93m必须输入一个要查询的字符串! \n \e[0m"
	return
fi
```