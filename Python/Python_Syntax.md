---
aliases: []
tags:
  - python
  - syntax
created: 2025-08-03 04:08:51
modified: 2025-08-25 23:30:41
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

#### <span id="syntax_basic_string">字符串</span>

##### 表达方式

Python 中字符串有几种表达方式，可以使用单引号 `'`、双引号 `"` 或三引号（`"""` 或 `'''`）。

##### 字符串索引

Python 字符串索引与 [C](../C/C_Note.md)、[Java](../Java/Java_Note.md) 从**0**开始。

### 复合数据类型

#### 列表

列表定义：

```python
a = [1,3,4]
```

Python 的列表类型特点：

1. 元素**可重复**
2. 元素排列**有序**

> [!tip] 
> 
> Python 的列表相当于 [Java](../Java/Java_Note.md) 中的 [数组](../Java/Java_Base_Note.md#数组)

#### 元组

元组与 [列表](#列表) 类似，但元组是「**只读**」的。其由一系列特定顺序排列的元素组成，不同在于元组一旦创建其**元素不能被修改**，算是一种「特殊的」列表，又称为「不可变列表」。

> [!info] 
> 
> 元组类似于 [Java](../Java/Java_Note.md) 中的 [数组](../Java/Java_Base_Note.md#数组)，但比数组还要「呆板」，Java 的数组只是数组这个容器大小，即数组的长度创建后不可变（因为数组是一块连续的内存，一旦分配了就无法更改），但数组中的元素是可以通过数组索引赋值的方式来修改的。而元组是完全不能对某个元素进行单独修改操作。

##### 创建元组

###### 通用创建语法

创建元组组，使用圆括号 `()`，元素间用逗号 `,` 区隔。圆括号 `()` 不是必需的，是**可省略**，当输出元组组时，Python 会自动加上一对圆括号。

语法：`变量名=(元素1，元素2,元素3,...,元素n)`

特殊语法

如果不向圆括号 `()` 传入任何元素，那么会创建一个空元组。

如果元组只包括**1**个元素，则需要在元素后面加逗号，否则圆括号会被当作运算符使用。

示例：

```python
t1=(1,3,6,8,10)

# 省略圆括号
t2=2,"hello",3.5

# 空元组
t3=()

# 只包括1个元素
t4=(8,)
# 或者省略括号，不过有点丑陋
t5=8,

```

###### 使用 `tuple` 函数创建

`tuple` 函数能够将其他数据结构对象转换成元组类型。其类型可以是 range 对象、[字符串](#syntax_basic_string)、元组或其他可迭代类型数据。

示例：

```python
tuple(range(5,15,2))
```

##### 元组操作

###### 访问元素

###### 截取

###### 连接与重复

###### 修改元素

元组一旦创建就不能修改其中的元素。但可以通过修改整个元组的变量来达到修改元素的目的。

示例：

```python
t1 = (100, 200)
# 打印出 100 200 
print(t1)
  
t1 = (20, 200)
# 打印出 20 200
print(t1)
```

> [!info] 
> 
> 示例中的 `t1`，前后两次「创建」，t1 这个元组的第一个元素就由 `100` 被「修改」为 `20`。这属于「曲线救国」式修改。

###### 删除元素

#### 集合

集合是由**不重复**元素组成的**无序**容器。

Python 的集合特点：

1. 元素**不可重复**
2. 元素排列**无序**

> [!tip] 关于列表和集合定义
>
> 在「类 C 语言」的标识符中，`[]`（中括号）暗含了「**有序性**」，即使用 `[]` 定义的数据类型，其中的元素是**有序**的。这种**有序性**有时体现在存储的有序，如 [C语言](../C/C_Note.md) 或 [Java](../Java/Java_Note.md) 中的 [数组](../C/C_Note.md#数组) 是使用 `[]` 来定义的；而有时这种**有序性**体现在「**可索引**」上，即可以通过索引来获取相应位置的元素。
> 
> 
> 而 `{}`（花括号）则隐含了「**无序性**」，即使用 `{}` 定义的数据类型，其中的元素是**无序**的。与**有序性**类似，使用 `{}` 定义的数据其**无序性**，有时体现在存储上，如 C、Java 等语言中 [函数](../C/C_Note.md#函数) 的「函数体」就是使用花括号来定义的，而可以谁其中的语句其存储上是**无序**的；而在集合类型中，这种「**无序性**」便体现在「不可索引」上，即不能通过索引值来获取相应位置的元素。

##### 创建集合

创建集合用花括号 `{}` 或 `set()` 函数。

示例：

```python
# 使用花括号创建集合
b = {1,3,4}

# 创建一个空的集合
b2 = set()
```

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
