---
aliases:
  - 
tags:
  - shell
  - linux
  - list
created: 2023-08-18 19:44:52
modified: 2023-09-18 18:17:17
---
# Shell 笔记

---
## 目录

---

##  <span id="shell_variable">变量</span>

### 特殊变量

* `$0`：当前脚本的文件名
* `$n`：传递给脚本或函数的参数。`n` 是数字，表示第几个参数，从**1**开始。
* `$#`：传递给脚本或函数的参数个数。
* `$*`：传递给脚本或函数的所有参数。
* `$?`：上个命令的退出状态，或函数的返回值。
* `$$`：当前 Shell 进程 ID。

---

## <span id="shell_expression">表达式</span>

### 条件表达式

语法：`[ expression ]`

**括号中的表达式前后都有空格**。
> [!tip] shell 的空格
> 
> 因为 Sehll 是命令加选项或参数而构成的，而命令与选项或参数间是以空格间隔的，所以在 Shell 中空格是有点「微妙」的存在。
> 
> 在条件表达式中，括号中的表达式前后必须都加上空格，不然就会报错。而赋值表达式中，`=` 前后就不能加空格。

#### 整数比较

* `-eq` 或 `equal`：等于
* `-ne` 或 `not equal`：不等于
* `-gt` 或 `greate than`：大于
* `-lt` 或 `lesser than`：小于
* `-ge` 或 `greate or equal`：大于等于
* `-le` 或 `lesser or equal`：小于等于

#### 字符串比较

* `==`：等于。`[ "a" == "a" ]`
* `!=`：不等于。`[ "a" != "a"]`
* `-n`：字符长度不等于 0 为真。`[ -n "xxx" ]`
* `-z`：字符串长度等于 0 为真。`[ -z "xxx" ]`

> [!tip] 字符串长度
> 
> 使用 `-n` 判断字符串长度时，变量加双引号。不但 `-n`，只要是字符串比较，都加双引号，这是一个好习惯。

#### 文件测试

* `-e`：文件或目录存在为真。`[ -e path ]`
* `-f`：文件存在为真。
* `-d`：目录存在为真。
* `-r`：有读权限为真。
* `-w`：有写权限为真。
* `-x`：有执行权限为真。

#### 布尔运算

* `!`：取反。`[ ! $a -eq $b ]`
* `-a`：和，类似于其他语言中的 `&` 运算符。`[ 1 -eq 1 -a 2 -eq 2]`
* `-o`：或，类似于其他语言中的 `|` 运算符。`[ 1 -eq 1 -o 2 -eq 2]`

#### 逻辑判断

* `&&`：断路与，与其他语言一样，前后俩表达式都为 true，其最终结果才为 true；如果前面的表达式为 false，那不用判断后面的表达式，最终结果即为 false。
* `||`：断路或，与其他语言一样，前后俩表达式至少有一为 true，结果才为 true；如果前面表达式为 true，则不用判断后面的表达式。
> [!tip]
> 
> 如果当前 Shell 不支持逻辑判断符，可使用 [布尔运算符](#布尔运算) 替代。

---

## <span id="shell_path">路径</span>

```shell
# 显示当前路径
echo $PWD
```
> [!tip] 
> 
> `$PWD`，跟 echo 命令一起使用时，必须大写。

---

## <span id="shell_array">数组</span>

取最后一个元素：`arr[-1]`。
> [!tip] 老语法
> 
> `${#arr[@]-1}`：`${#arr[@]}` 这是取数组长度，那数组长度 -1 就得到数组最后一个元素的索引值。

---

## <span id="shell_string">字符串</span>

### 截取

```shell

# # 是从左向右，一个#是取第一个，##是取最后一个
# % 是从右向左 一个%是取第一个，%%是取最后一个
## 
s1="https://github.com/rexdf/ChineseLocalization.git"

# 从左向右取第一个.后的字符串
# 即取到的是 com/rexdf/ChineseLocalization.git
echo "${s1#*.}"

# 从左向右取最后一个.后的字符串
# 即取到的是 git
echo "${s1##*.}"

# 从右向左取.后的第一个字符串
# 即取到的是 https://github.com/rexdf/ChineseLocalization
echo "${s1%.*}"

# 从右向左取最后一个.后的字符串
# 取到的是 http://github
echo "${s1%%.*}"

```

---

## 条件

```shell
if [ command ];then
     符合该条件执行的语句
elif [ command ];then
     符合该条件执行的语句
else
     符合该条件执行的语句
fi
```

---

## 循环

#### 示例

##### 循环读取文件

`while` 与 `for` 读文件是有区别的：
1. `while` 是逐行读取，读完一行跳转到下一行
2. `for` 是按字符串方式读取，遇到空格后，再读取的数据就会换行显示

`while` 相对于 `for` 的读取能更好的还原数据原始性。

###### 相关资料

* [SHELL 读取文件的每一行内容并输出 | 菜鸟教程](https://www.runoob.com/w3cnote/shell-read-line.html)
* [shell读取文件](https://blog.csdn.net/qq_26620783/article/details/87430195)

###### for 实现

```shell
for line in `cat xxx.txt`
do
	echo $line
done
```

###### while 实现

写法 1：

```shell
cat xxx.txt | while read line
do
  echo $line
done
```

写法 2：

```shell
while read line
do
  echo $line
done < xxx.txt
```

---

## 相关工具

### shfmt

[shfmt](https://github.com/mvdan/sh) 是一款 shell 脚本格式化工具。

这工具可以与多款 [文本编辑器](../Editors/Editors_Note.md) 的 shell 格式化插件配合使用。
> [!info] 
> 
> [各编辑器 shell 格式化插件列表](https://github.com/mvdan/sh#related-projects)

```shell
shfmt -l -w script.sh
```

---

## 视频清单

* [2023年最通俗易懂的Shell教程](https://www.bilibili.com/video/BV1Tu411W7Up)
* [2023年最新Shell自动化开发全套顶级天花板教程](https://www.bilibili.com/video/BV1494y1i7ca)

