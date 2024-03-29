---
aliases:
  - 
tags:
  - perl
  - PL
created: 2023-08-15 20:23:04 
modified: 2023-09-15 20:25:44

---

# Perl 笔记

---

## 目录

---

## CPAN

[The Comprehensive Perl Archive Network](https://www.cpan.org/) 是一个 Perl 的第三方模块库。

## 基础语法

### 严格模式

```perl
use strict;
```

严格模式下，语法有更严谨的规定，如语句得使用分号 `;` 结束等。

### 变量

在 [严格模式](#严格模式) 下，本地变量得在变量声明或定义时加 `my`，声明此变量为本地变量。

```perl
my $num1=1;
```

#### 标量

#### 数组

数组是用于存储一个有序 [标量](#标量) 值的变量。

数组变量名使用 `@` 来标识。

```perl
@ages = (28,30,50)
```

数组本质是一个特殊的 [标量](#标量)，所以在访问数组中的各元素时，其实是访问一个个 [标量](#标量)，所以访问数组时，得使用 `$`。

```perl
print "$ages[0]"
```

perl 中的数组索引同样是从 `0` 开始。

将数组赋值给一个 [标量](#标量)，这个标量拿到的是这个数组的长度：
```perl

$num = @ages;
# 显示是ages数组的长度
print $num;
```

#### 哈希

## 文档资料

* [CPAN - 哔哩哔哩](https://www.bilibili.com/read/cv14880480)

