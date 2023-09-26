---
aliases:
  - 
tags:
  - 
created: 2023-09-23 01:34:32
modified: 2023-09-23 01:37:38
---
# Shell 脚本示例笔记

---

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
