---
aliases: []
tags:
  - shell
  - linux
  - list
created: 2023-08-18 19:44:52
modified: 2024-01-01 02:31:22
---
# Shell 笔记

---
## 目录

---

##  <span id="shell_variable">变量</span>

### 局部变量

默认在 shell 脚本中定义的变量都是「全局」（global）的。

使用 `local` 关键字修饰的变量，称为「局部变量」，其作用域只在 [函数](#函数) 范围。

如果出现同名，shell 函数中定义的 local 变量会屏蔽掉脚本定义的全局变量。

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

### 数组初始化

```shell
数组名=()
```

### 数组元素

#### 数组所有元素

* `*`：访问全部元素，是作为一个整体
* `@`：同上，但会强制单词分隔
  > [!tip] for in 中使用
  > 
  > `*` 与 `@` 区别在 `for in` 循环中会体现出区别：
  > 
  >
  >> ```shell
  >>  arr1=("hello world" "the man")
  >>  
  >>  for i in "${arr1[*]}";do
  >>  echo $i
  >> done
  >>```
  >
  >  这段代码会输出 `hello world then man`
  >  
  >> ```shell
  >>  for i in "${arr1[@]}";do
  >> echo $i
  >> done
  >>``` 
  > 
  >  这段代码会输出：
  >  
  >> ```shell
  >>  hello world
  >>  the man
  >> 
  >>``` 
  >
  > 

取最后一个元素：`arr[-1]`。
> [!tip] 老语法
> 
> `${#arr[@]-1}`：`${#arr[@]}` 这是取数组长度，那数组长度 -1 就得到数组最后一个元素的索引值。

`${!arr[*]}` 或 `${!arr[@]}` 是查看已赋值元素的下标。

### 示例

#### 1. 读取文件并将数据存放到数组中

```shell
# 地址数组
addrs_arr=()

# 读取文件
# 去除空行及以 # 起头的行
for line in `cat $addrsls_file_path | grep -v ^$ | grep -v ^\#` 
do
	# 将数据存放到数组
	addrs_arr+=("$line")
done

```

#### 2. 遍历数组

```shell
# 遍历数组
for arr_temp in ${addrs_arr[*]}
do
	echo $arr_temp
done

```

> [!tip] 遍历说明
> 
> `arr_temp` 是一个「临时变量」，用来存储每次循环从数组中取出的数据。
> 
> 这示例中的 `for` 循环类似其他高级编程语言中的 `foreach` 语句的用法。

#### 4. 函数中的数组使用

##### 将一个数组作为参数向函数传递

```shell
fun1 ${ads_arr[*]}
```

> [!tip] 数组实参
> 
> 在调用一个函数时，向此函数传入一个「实参」时，如果是普通变量只需要 `$变量名`，就可以。
> 
> 但如果要向函数传个数组，那语法就得「变下」，得写成 `${数组名[*]}` 或 `${数组名[@]}`。如果数组变量按普通变量写法传参，那只会传入数组第一个元素，即 `${数组名[0]}`。
> 
> 其实这是符合数组获取 [数组所有元素](#数组所有元素) 的语法规则的。简单说，shell 语法与其他高级编程语言有点不一样，传参得「实打实」地传入数组的「所有元素」。

##### 函数内接收外部传入的数组

```shell
# 接收外部传来的数组
local ads_array=($@)
```

> [!tip] 语法解释
> 
> 函数中接收传入的数组，其语法也与接收普通变量不一样，函数内接收普通变量参数可以这样：`temp=$数字`，通过数字来指定接收哪个一参数。
> 
> 而因为数组不是一个数据，而是「一堆」数据，所以得使用特殊一点语法接收，其中 `@` 跟 [数组所有元素](#数组所有元素) 的语法保持一致，另外，那对小括号 `()`，其实是就是构建一个数组的语法。即 `ads_array=()` 这个是一个 [数组初始化](#数组初始化) 语法。
> 
> 也就是说，函数内接收数组，其实是初始化了一个数组用来接收。
> 

> [!info] 相关资料
> 
> * [shell 数组与函数之间的传参 - 知己一语 - 博客园](https://www.cnblogs.com/zhijiyiyu/p/15038939.html)

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

## <span span id="shell_function">函数</span>

函数定义语法：

```shell
function 函数名(){
	# 函数体
}
```

> [!info] 语法解释
> 
> 跟 [Javascript](../JS/JS_Note.md) 等语言很像。

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
* [Shell脚本从入门到实战](https://www.bilibili.com/video/BV1k94y1W7cJ)

---

## 其他相关笔记

* [Linux笔记](Linux_Note.md)
* [Shell示例笔记](Shell_Example.md)

