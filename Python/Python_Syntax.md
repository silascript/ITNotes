---
aliases: []
tags:
  - python
  - syntax
created: 2025-08-03 04:08:51
modified: 2025-08-03 04:16:43
---

# Python 语法笔记

---

## <span id="syntax_basic">基础语法</span>

### <span id="syntax_basic_varexp">变量表达式</span>

#### 变量名规则

* 变量名只能包含字母、数字和下划线。不能以数字开头。
* 不能包含空格
* 不要用 Python 关键字和函数名或保留字作变量名

> [!tip] 
>
>慎用小写字母**l**和大写字母**O**，因为可能与数字**1**和**0**混淆。

### <span id="syntax_basic_type">简单数据类型</span>

#### <span id="syntax_basic_">数值类型</span>

##### <span id="syntax_basic_int">整型</span>

##### <span id="syntax_basic_float">浮点型</span>

##### <span id="syntax_basic_complex">复数</span>

#### <span id="syntax_basic_">字符串</span>

### 复合数据类型

#### 列表和集合

##### 列表

列表定义：

```python
a = [1,3,4]
```

Python 的列表类型特点：

1. 元素**可重复**
2. 元素排列**有序**

##### 集合

集合定义：

```python
b = {1,3,4}
```

Python 的集合特点：
1. 元素**不可重复**
2. 元素排列**无序**

> [!tip] 关于列表和集合定义
>
> 在「类 C 语言」的标识符中，`[]`（中括号）暗含了「**有序性**」，即使用 `[]` 定义的数据类型，其中的元素是**有序**的。这种**有序性**有时体现在存储的有序，如 [C语言](../C/C_Note.md) 或 [Java](../Java/Java_Note.md) 中的 [数组](../C/C_Note.md#数组) 是使用 `[]` 来定义的；而有时这种**有序性**体现在「**可索引**」上，即可以通过索引来获取相应位置的元素。
> 
> 
> 而 `{}`（花括号）则隐含了「**无序性**」，即使用 `{}` 定义的数据类型，其中的元素是**无序**的。与**有序性**类似，使用 `{}` 定义的数据其**无序性**，有时体现在存储上，如 C、Java 等语言中 [函数](../C/C_Note.md#函数) 的「函数体」就是使用花括号来定义的，而可以谁其中的语句其存储上是**无序**的；而在集合类型中，这种「**无序性**」便体现在「不可索引」上，即不能通过索引值来获取相应位置的元素。

#### 元组

#### 字典

### 函数

### 类

### 文件

---

## <span id="syntax_advanced">进阶语法</span>

### 多任务

### 网络

### 图形界面

---

## <span id="python_style">风格规范</span>

### <span id="python_style_pep8">PEP8</span>

#### PEP8 相关链接

* [PEP 8 – Style Guide for Python Code](https://peps.python.org/pep-0008/)
* [PEP 8 中文](https://blog.csdn.net/ratsniper/article/details/78954852)
* [PEP 257 – Docstring Conventions | peps.python.org](https://peps.python.org/pep-0257/)

### <span id="python_style_google">Google 规范风格</span>

#### Google 规范相关链接

* [Google Python 风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)
* [Google Python 编码规范 中文版](https://www.runoob.com/w3cnote/google-python-styleguide.html)
* [Google Python 编码规范 英文版](https://google.github.io/styleguide/pyguide.html)

---

## 相关笔记

* [Python 笔记](Python_Note.md)
* [Python 资料清单](Python_Material.md)
* [Python 视频](Python_Videos.md)
