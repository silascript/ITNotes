---
aliases: []
tags:
  - linux
  - shell
  - awk
created: 2024-09-19 21:43:32
modified: 2025-11-19 21:09:37
---

# AWK 笔记

---

## 字段

`awk` 中的「字段」，指的是一行中的列，列与列之间用特定的符号隔开（分隔符）。

> [!info] 
> 
> 默认情况下，`awk` 的字段分隔符是空白字符。可以通过 `-F` 参数显式地对分隔符进行设置。

## 记录

---

## 变量

`awk` 中有两种变量：用户自定义变量和 [内建变量](#内建变量)。

### 内建变量

* `NF`：当前记录的字段数
* `FS`：字段分隔符

## 数组

---

## 示例

### 路径和文件相关

#### 统计 [zip](Linux_Note.md#zip) 包中有多少个一级目录

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

#### 打印路径中最近的目录

```shell
echo "~/MyNotes/ITNotes/常用字体.txt" | awk -F '/' '{print $(NF-1)}'
```

结果：

```shell
ITNotes
```

> [!info] 
> 
> `-F` 是指定分隔符，默认是 `:` 来分隔的
> 
> `$NF` 是最后一列，而倒数第二个就是 `$(NF-1)`

---

## 相关笔记

* [Linux 笔记](Linux_Note.md)
* [Shell 笔记](Shell/Shell_Note.md)
* [AWK 资料清单](AWK_Material.md)

