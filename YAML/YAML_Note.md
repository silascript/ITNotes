---
aliases: []
tags:
  - yaml
  - yml
created: 2023-08-18 19:44:52
modified: 2024-09-27 02:34:41
---

# YAML 笔记

---

YAML 是一种数据序列化语言，用于方便的读写配置文件，属于 `JSON` 的超集。

YAML 语言规则如下：

* 大小写敏感
* 键值之间（冒号后）必须有至少一个空格
* 使用空格缩进表示层级关系，不允许使用制表符
* 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
* 文件使用 `.yaml` 或 `.yml` 后缀结尾
* 使用 `#` 表示注释
* 每个冒号后都要有一个空格，第一个参数名称必须需要打头写

## 数据结构

### 纯量​

纯量，也称「字面量」，是最基本的、**不可再分**的值，即基本的数据类型：

* 字符串
* 整数
* 浮点数
* 布尔值
* Null
* 日期
* 时间

#### 字符串

```yaml

# 单行字符串
name: Liang
description: 'Hello, World!'

# 双引号不会对特殊字符转义
# { "s1":"内容\\n字符串", "s2":"内容\n字符串" }
s1: '内容\n字符串'
s2: "内容\n字符串"

# 字符串换行会转换成 1 个空格
# { "address":"Shanghai, China" }
address: Shanghai,
  China
```

> [!tip] 
> 
> yaml 字符串默认不使用引号表示。如果字符串之中包含空格或特殊字符，需要放在引号之中。
> 
> yaml 中的字符串引号，正好与 [Shell](../Linux/Shell/Shell_Note.md) 相反，它是双引号 `"` 不转义，单引号 `'` 转义。

### 对象

```yaml
person:
  name: 陈皮
  age: 18
  man: true
```

也可以写成这样：

```yaml
person: {name: 陈皮, age: 18, man: true}
```

### 数组

用**短横杆**加**空格** `-` 开头的行，组成数组的每一个元素：

```yaml
address:
  - 深圳
  - 北京
  - 广州
```

当然也可以写成这样：

```yaml
address: [深圳, 北京, 广州]
```

#### 数组里嵌套数组

```yaml
twoArr:  
    -  
      - 2  
      - 3  
      - 1  
    -  
      - 10  
      - 12  
      - 30
```

#### 数组中嵌套对象

```yaml
childs:
  -
    name: 小红
    age: 10
  -
    name: 小王
    age: 15
```

或者写成这样：

```yaml
childs:
  - name: 小红
    age: 10
  - name: 小王
    age: 15
```

或缩成一行：

```yaml
childs: [{name: 小红, age: 10}, {name: 小王, age: 15}]
```

---

## 相关资料

* [YAML 资料清单](YAML_Material.md)