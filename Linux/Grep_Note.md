---
aliases: []
tags:
  - linux
  - grep
created: 2024-08-25 10:24:29
modified: 2024-08-25 10:26:41
---

# Grep 笔记

---

## 简介

grep 全称是 「**global search regular expression and print out the line**」，翻译过来就是 **全局搜索正则表达式并把行打印出来**。

grep 是一个程序族，包括了 **grep**、**egrep** 和 **fgrep**。

Linux 中使用的 GNU 版本的 grep ，可以直接通过 `-G`、`-E` 和 `-F` 命令选项来使用 grep、egrep 和 fgrep 的功能。

## 常用参数

* `-v` 或 `--invert-match`：反向查找，即打印出*不符合条件行*的内容。

## ripgrep

[ripgrep](https://github.com/burntsushi/ripgrep) 是使用 [Rust](../Rust/Rust_Note.md) 编写的现代版 [grep](#grep)。

## 正则表达式

* `^`：锚定行的开始。如：`^grep` 匹配所有以 grep 开头的行。    
* `$`：锚定行的结束。如：`grep$` 匹配所有以 grep 结尾的行。
* `.`：匹配**一个非换行符**的字符。如：`gr.p` 匹配 gr 后接一个任意字符，然后是 p。    
* `*`：匹配**零个或多个**先前字符。如：`*grep` 匹配所有一个或多个空格后紧跟 grep 的行。    
* `.*`：一起用代表**任意**字符。   
* `[]`：匹配一个**指定范围内**的字符，如 `[Gg]rep` 匹配 Grep 和 grep。    
* `[^]`：匹配一个**不在指定范围内**的字符，如：`[^A-Z]rep` 匹配不包含 A-Z 中的字母开头，紧跟 rep 的行
* `\(..\)`：**标记匹配**字符。如 `\(love\)`，love 被标记为 1。    
* `\<`：锚定**单词的开始**。如：`\<grep` 匹配包含以 grep 开头的单词的行。    
* `\>`：锚定**单词的结束**。如 `grep\>` 匹配包含以 grep 结尾的单词的行。    
* `x\{m\}`：重复字符 x，m 次。如：`o\{5\}` 匹配包含 5 个 o 的行。    
* `x\{m,\}`：重复字符 x,至少 m 次。如：`o\{5,\}` 匹配至少有 5 个 o 的行。    
* `x\{m,n\}`：重复字符 x，至少 m 次，不多于 n 次。如：`o\{5,10\}` 匹配 5~10 个 o 的行。   
* `\w`：匹配**文字和数字**字符，也就是 [A-Za-z0-9]。如：`G\w*p` 匹配以 G 后跟零个或多个文字或数字字符，然后是 p。   
* `\W`：`\w` 的反置形式，匹配**一个或多个非单词**字符，如点号句号等。   
* `\b`：单词锁定符。如: `\bgrep\b` 只匹配 grep。  

---

## 相关笔记

* [Linux 笔记](Linux_Note.md)