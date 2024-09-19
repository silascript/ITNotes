---
aliases: []
tags:
  - linux
  - shell
  - awk
created: 2024-09-19 21:43:32
modified: 2024-09-19 21:50:38
---

# AWK 笔记

---

统计 [zip](Linux_Note.md#zip) 包中有多少个一级目录：

```shell
zipinfo $zip_file | grep "^d" | awk '{print $9}' | awk 'BEGIN{FS="/"}{print $1}' | uniq | wc -l
```

> [!info] 解释代码各步骤
> 
> 1. `grep "^d"` 先把所有目录都筛出来
> 2. `awk '{print $9}'` 把最后那段，即目录路径那个字段筛出来（按默认的空格分隔）
> 3. `awk 'BEGIN{FS="/"}{print $1}'`：改变分隔符为 `/`，就是路径分隔符，把一级目录与其目录分离并取出一级目录
> 4. `uniq`：去重，把相同名称的（一级）目录去掉重复
> 5. `wc -l`：统计数量

---

## 相关笔记

* [Linux 笔记](Linux_Note.md)
* [Shell 笔记](Shell/Shell_Note.md)

