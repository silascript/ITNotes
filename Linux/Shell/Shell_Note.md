---
aliases: []
tags:
  - shell
  - linux
  - list
  - sh
  - bash
  - zsh
created: 2023-08-18 19:44:52
modified: 2024-08-11 02:15:19
---

# Shell 笔记

---
## 目录

---

## <span id="shell_type">Shell 种类</span>

### Bourne shell 

> Bourne shell 由 Steve Bourne 在 AT&T 贝尔实验室开发，被认为是第一个 UNIX shell。它被表示为 sh。 #sh

### Bash

> GNU Bourne-Again Shell (bash) 更多被称为 Bash shell，它被设计成与 Bourne shell 兼容。Bash shell 融合了 Linux 中不同类型 shell 的有用功能，如 Korn shell 和 C shell。 #bash

### zsh

> Z Shell 或 zsh 是 sh shell 的扩展，在自定义方面做了大量改进。如果你想要一个具有更多功能的现代 shell，zsh shell 就是你要找的。 #zsh

### 相关资料

* [Linux 中有哪些不同类型的 Shell？ - 知乎](https://zhuanlan.zhihu.com/p/614551538)

---

## <span id="shell_mode">Shell 模式</span>

Linux 下常见有：`.bashrc`、`.profile`、`.bash_profile` 等配置文件。

这些配置文件的区别，首先得从 shell 的 4 种使用模式讲起。

#### 交互与非交互

##### 交互模式

顾名思义就是 shell 与用户存在交互行为。 #shell/mode 

##### 非交互模式

与 [交互模式](#交互模式) 的情况就正好相反。 #shell/mode

> [!note] 
> 
> [交互模式](#交互模式) 与 [非交互模式](#非交互模式)  区别的是 bash/zsh 用于接受用户命令，还是执行运行一段脚本。

#### 登录与非登录

使用 `echo $0` 可以检查是否是登录 shell：
* 如果显示结果是 shell 名称前有一个「**连字符**」`-`,即登录 shell。
* 如果没有「**连字符**」，即非登录 shell。 

要登录的 shell，无论是否交互，最后都得加载 `.profile` 文件。

而如果是

示例：

使用 ssh 连接本机：

```shell
$ ssh silascript@192.168.0.20

# silascript @ (base) in ~ [4:57:15] 
$ echo $-
569XZhilms

# silascript @ (base) in ~ [4:57:25] 
$ echo $0
-zsh
```

> [!note] 
> 
> 可以看到 `-zsh`，这是有连字符 `-` 的，说明此时的 shell 是一个 [登录shell](#登录与非登录)。
> 
> 另外，`$-` 的结果是 `569XZhilms`，其中存在 `i`，证明这个 shell 还是个 [交互shell](#交互与非交互)。
>
>> [!tip] 
>> 而如果只是直接通过本地的各种终端直接输入 `echo $0`，一般都是直接显示 shell 的路径，如 `/bin/zsh` 这样的。`echo $-` 结果与通过 ssh 方式连接结果一致。由此可知，在图形界面的 Linux 下，打开一个终端窗口，这种方式的 shell 都是一个非登录式的 shell。

#### shell 类型总结

* 登录式 shell：
	* 正常通过某终端登录的 shell。
	* su - username。
	* su -l username。

* 非登录式 shell：
	* su username。
	* **图形终端**下打开的命令窗口。
	* 自动执行的 shell 脚本。



Sell 语法相关的内容请查看： [Shell 笔记](Shell_Note.md)

---

## <span id="shell_tools">Shell 工具</span>

### Bash-It

* [bash-it](https://github.com/Bash-it/bash-it.git) 是 [Bash](#Bash) 的配置框架，可以认为是 bash 版本的 [oh-my-zsh](zsh_note.md#zsh_plugins_mgs_ohmyzsh)。

---

##  <span id="shell_variable">变量</span>

命名规则：

* 首个字符必须为字母
* 中间不能有 [空格](#空格)，可以使用下划线 `_`
* 不能使用标点符号
* 不能使用 [Bash](#Bash) 等 Shell 里的关键字

### 空格

shell 对于空格有严格的规定：

* **赋值**语句等号两边**绝对不能有**空格。
* [字符串比较](#字符串比较) 等号两边**必须有**空格。
* 所赋的值包含空格，可以用引号括起来。
* `if` 语句的 `[]` 中，表达式前后都应有空格。而 `if` 语句中那个 `;` 与 `]` 间不能有空格。
	> [!example] 小示例
	> ```shell
	> if [ "${s1}" = "hello" ];then
	> ```

> [!info] 相关资料
> 
> * [变量定义规则、shell 格式、空格注意事项汇总](https://blog.csdn.net/m0_45406092/article/details/129047592)

### 引号

数字可带可不带引号，但不带引号的数字可计算，带引号的数字不能用于计算。

> [!info] 相关资料
>
> * [单引号、双引号、不加引号和反引号用法和区别详解](https://blog.csdn.net/m0_45406092/article/details/129056037)

### 局部变量

默认在 shell 脚本中定义的变量都是「全局」（global）的。

使用 `local` 关键字修饰的变量，称为「局部变量」，其作用域只在 [函数](#函数) 范围。

如果出现同名，shell 函数中定义的 local 变量会屏蔽掉脚本定义的全局变量。

### 特殊变量

* `$0`：当前脚本的文件名
* `$n`：传递给脚本或函数的参数。`n` 是数字，表示第几个参数，从**1**开始。
* `$#`：传递给脚本或函数的参数个数。
* `$*`：传递给脚本或函数的所有参数，即以一个单字符显示所有向脚本传递的参数。
* `$?`：上个命令的退出状态，或函数的返回值。`0` 表示没有错误，其他值表明有错误。
* `$$`：当前 Shell 进程 ID。
* `$!`：后台运行的最后一个进程 ID 号
* `$@`：与 `$*` 相同，但是使用时加引号，并在引号中返回每个参数。
* `$-`：显示 Shell 使用的当前选项，与 `set` 命令功能相同。

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

```shell
 if [[ xxx ]];then
	xxx
elif [[ xxx ]];then
	xxxx
else
	xxxx
fi
```

> [!tip] 
> 
> Shell 中是 `elif`，不是 else if

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

   > [!info] 示例
   > 
   > ```shell
   >  if [[ ! -d "/data/" ]];then
   > 	  mkdir /data
   > fi
   > ```
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

### 相关资料

* [Shell脚本中if条件判断的写法实例\_linux shell\_脚本之家](https://www.jb51.net/article/235932.htm#_lab2_3_2)
* [linux,shell中if else if的写法,if elif - Zhai\_David - 博客园](https://www.cnblogs.com/chuanzhang053/p/8566043.html)

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

## <span id="shell_list">列表</span>

列表实际就是有空格的字符串。

构建一个列表：

```shell
list1=("aa" "bb" 5 "cc")
```

获取列表元素，从 `0` 开始：

```shell
echo ${list1[0]}
```

获取所有列表元素：

```shell
echo ${list1[@]}
```

遍历列表：

```shell
for s_temp in "${list1[@]}"; do
	echo $s_temp
done
```

获取列表长度：

```shell
echo ${#list1[@]}
```

---

## <span id="shell_array">数组</span>

### 数组初始化

```shell
数组名=()
```

### 数组元素

#### 数组所有元素

* `*`：全部元素，是作为**一个整体**处理
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
  >>```  - 

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
> `${#arr[@]-1}`：`${#arr[@]}` 这是取数组长度（这跟 [列表](#列表) 是一样的。），那数组长度 -1 就得到数组最后一个元素的索引值。

`${!arr[*]}` 或 `${!arr[@]}` 是查看已赋值元素的下标。

> [!info] 
> 
> `@` 跟 `*` 的区别：
> 
> * 变量使用 `*` 时，变量被 `""` 包裹，会当成一串字符串处理。
> * 变量使用 `@` 时，变量被 `""` 包裹，依然当做数组处理。
> * 变量在没有被 `""` 包裹的情况下，`@` 跟 `*` 是等效的.

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

> [!info] 
> 
> `grep -v` 表示反向选择。
> 
> `^$` 与 `^\#` 都是 [正则表达式](Linux_Note.md#正则表达式) 的东西。`^$` 表示空行，`^\#` 表示以 `#` 符号开始的行。
>
>`grep -v ^$ | grep -v ^\#` 表示就是选项非空行及非使用 `#`「标记」的行。

#### 2. 遍历数组

方式 1：

```shell
# 遍历数组
for arr_temp in ${addrs_arr[*]}
do
	echo $arr_temp
done

# 或者写成这样
for arr_temp in ${addrs_arr[@]}
do
	echo $arr_temp
done

```

> [!tip] 遍历说明
> 
> `arr_temp` 是一个「临时变量」，用来存储每次循环从数组中取出的数据。
> 
> 这示例中的 `for` 循环类似其他高级编程语言中的 `foreach` 语句的用法。

方式 2：

```shell
for i in ${!json_data_arr[@]}
do
	echo ${json_data_arr[$i]}
done
```

> [!tip] 
> 
> 使用 `!` 这种方式，是使用数组索引来遍历。
>  
> `i` 是数组索引

#### 3. 添加元素

向数组中添加元素，可以有四种方法：

1. 按照下标进行单个添加
```shell
array_name[index]=value
```

2. 在不做任何删减时，直接使用数组长度追加元素
```shell
array_name[${#array_name[@]}]=value
```

3. 直接获取源数组的全部元素再加上新要添加的元素，一并重新赋予该数组，重新刷新定义索引
```shell
array_name=("${#array_name[@]}" value1 value2 ... valueN)
```
> [!info] 
> 
> 双引号不能省略，否则数组中存在包含空格的元素时会按空格将元素拆分成多个。
> 
> 不能将 `@` 替换为 `*`，如果替换为 `*`，不加双引号时与 `@` 的表现一致，加双引号时，会将数组 array_name 中的所有元素作为一个元素添加到数组中。这规则在 [字符串](#字符串) 中也同样适合。

4. 使用 `+=` 直接添加，待添加元素必须用 `()` 包围起来，并且多个元素用空格分隔
```shell
array_name+=(value1 value2 ... valueN)
```
> [!important] 
> 
> 待添加元素必须用 `()` 包围起来，并且多个元素用空格分隔
> 
>
> `()` 对于数组而言，是**构建数组的关键**。如获取一相函数返回的数组，如果没有 `()`，将被 Shell 当成字符串处理，这时使用 `${#array[@]}` 来获取数组长度将只会是 `1`，因为虽然函数使用 `echo ${array[@]}` 返回了一个数组，但接收方会将这货当成一个字符串处理，因为 Shell 中实际都是字符，因为 Shell 或 [Linux](Linux_Note.md) 大部分工具，实质是都是针对处理文本而生的，所以字符或字符串才是其本质。
> 
> ```shell
> function test1(){
> 	local array1=(2 5 22 32 18)
> 	echo ${array1[@]}
> }
> # 获取返回值次构建为数组
> r_arr=($(test1))
>
> echo ${#r_arr[@]}
> ```
> 

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

#### 5. 判断数组是否为空

##### 方式 1

通过 `${#数组[@]}` 语法获取数组元素的个数来判断：

```shell
arr1=()
if [ ${#arr1[@]} -eq 0 ];then
	echo "数组为空"
else
	echo "数组不为空"
fi

```

##### 方式 2

通过 `${数组[@]}` 语法获取数组所有元素，如果返回一个空值，则表明此数组为空：

```shell
arr1=()
isEmpty=true
for element in "${arr1[@]}"; do
    isEmpty=false
    break
done

if $isEmpty; then
    echo "Array is empty"
fi
```

> [!info] 
> 
> 最「笨」的方式就是遍历数组，设个结果变量，判断每一个元素。

---

## <span>运算</span>

原生 Shell 不支持数学运算，可以通过 [awk](Linux_Note.md#linux_textprocessing_awk)、`expr` 命令来实现。

```shell
num1=`expr 1+2`
echo num1

```

当然还有更便捷的方式，使用 `(())` 来进行整数运算。

---

## <span id="shell_string">字符串</span>

### 截取

#### 示例 1

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

> [!tip] 
> 
> 快速记忆是从左还是从右，可以这么记：键盘上 `#` 在左，而 `%` 在右边，所以 `#` 是从左向右，`%` 是从右向左。

#### 示例 2

```shell

# core_address是 denolehov/obsidian-git 这个样子。获取 / 左右两段字符串

# 取前段 使用从右向左取，取 / 最后一段
local account=${core_address%%/*}
# 取后牌戏 使用从左向右取，同样取 / 最后一段
local p_name=${core_address##*/}

```

> [!tip] 
> 
> 从左往右，`*` 与 `#` 在一起，而且 `*` 总在左边
> 
> 从右往左，`*` 与 `#` 总被分隔符分离，`*` 总在右边。

### 遍历

字符串是可以遍历的。

#### 示例 1

```shell
str2="hello,world"

for s_temp in $str2; do
	echo $s_temp
done

```

> [!info] 
> 
> 示例 1 中最终输出就是 `hello,world`，就是把字符串中每一个字符「遍历」一遍。

#### 示例 2

```shell
str1="silas tom jack lucy mary"

for s_tem in $str1; do
	echo $s_tem
done

```

> [!info] 
> 
> 示例 2 中的字符串，是带空格的；所以遍历的结果是按空格分割，将分割后每段子串依次输出。
> 
> 这种以空格为分隔符的字符串，也可以这样遍历：
> 
>> ```shell
>> for s_tem in ${str1[@]}; do
>> 	 echo $s_tem
>> done
>> ```
>
> 其实这是一种 [数组样式](#2.%20遍历数组) 的遍历。

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

###### for 实现

```shell

for 变量名 in 循环列表
do
  命令集
done
    
```
> [!info] 
> 
> 这种 `for` 循环语句语法中，`for` 关键字后面会有一个「变量名」，变量名依次获取 `in` 关键字后面的变量取值列表内容（以空格分隔），每次仅取一个，然后进入循环（`do` 和 `done` 之间的部分）执行循环内的所有指令，当执行到 `done` 时结束本次循环。
> 
> 之后，「变量名」再继续获取变量列表里的下一个变量值，继续执行循环内的所有指令，当执行到 `done` 时结束返回，以此类推，直到取完变量列表里的最后一个值并进入循环执行到 `done` 结束为止。

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
while read line$*用法
do
  echo $line
done < xxx.txt
```

##### 小示例

这示例使用到了 [jq](#jq) 这个 json 小工具，对 json 文件进行解析，并将解析后的结果数据输出存放到一个临时文件中。

然后对这个临时文件进行读取，在读取时，还对读到的数据进行一定需求的过滤。

```shell
function get_dl_url(){

	# json文件
	local json_path=$1

	# 使用 jq 获取各文件下载地址并输出到临时文件中
	jq -r '.assets[] | .browser_download_url' $json_path > temp.txt

	# 过滤掉 main.js manifest.json styles.css 三个文件之外所有文件
	for line in `cat temp.txt`
	do
		# 从文件地址中获取文件名
		local f_name=${line##*/}
		# 过滤文件
		if [[ $f_name == "main.js" ]] || [[ $f_name == "manifest.json" ]] || [[ $f_name == "styles.css" ]];then
			echo $line
		fi
	done
}
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
> 跟 [Javascript](../../JS/JS_Note.md) 等语言很像。

### <span id="shell_function_parameter">参数</span>

Shell 脚本内，传递参数格式为 `$n`，**1**为执行脚本的第一个参数，**2**为执行脚本的第二个参数，以此类推。

* `$#`：传递到脚本的参数个数
* `$*`：以一个单字符串显示所有向脚本传递的参数。
> [!tip] 
> 
> 如果使用引号 `"` 括起来，是以 `"$1 $2 ... $n"` 形式输出所有参数。
* `$@`：与 `$*` 相同，但是使用时加引号，并在引号中返回每一个参数。
> [!tip] 
> 
> 如果使用引号 `"` 括起来，是以 `"$1" "$2" ... "$n"` 形式输出所有参数。
> 
> `$*` 与 `$@` 跟 [数组元素](#数组元素) 中的 `*` 和 `@` 类似，`$*` 是把参数当成一个「**整体**」处理，而 `$@` 是单个参数的组合。
* `$$`：脚本运行的当前进程 ID 号。
* `$!`：后台运行的最后一个进程 ID 号。
* `$?`：显示最后命令的退出状态。`0` 表示没有错误，其他任何值表明有错误。

### <span id="shell_function_returnv">返回值</span>

#### 返回及获取

Shell 返回值可以有两种方式进行返回：

1. 使用 `echo` 进行返回。使用这种方式返回的返回值都是字符串，获取方式可以是使用变量接收 `$(xxx)` 函数执行结果，也可以通过 `$?` 方式获取。
2. 使用 `return` 进行返回，这种方式只能返回整型。这种方式只能通过 `$?` 方式获取。

##### 示例

```shell
# 检测目录是否存在
# 返回值：0为存在 其余都是有问题的
function validate_dir() {

	local dir_path=$1

	# 检测路径是否为空
	if [ -z $dir_path ]; then
		echo -e "\e[93m Vault路径不能为空！\n \e[0m"
		return 1
	fi

	# 目录存在
	if [ ! -d "$dir_path" ]; then
		echo -e "\e[93m $dir_path \e[96mVault路径不存在！\n \e[0m"
		return 1
	else
		# return 0
		echo 0
	fi

}

# 检测 Vault 路径
r_1=$(validate_dir $1)
echo $r_1
# echo $?

```

> [!tip] 
> 
> 示例中，因为需要判断后就结束函数，所以 `return` 是必须的，同时还要显示「错误」信息。
>
> 这样写，即满足业务需求，而且还能兼容 `r_1=$(validate_dir $1)` 及 `echo $?` 这两种获取返回值的方式。

#### 返回数组

其实函数所有返回值无论之前是什么类型，最后都是以 [字符串](#字符串) 形式返回。

如果是返回 [数组](#数组)，实际返回一个带有空格的字符串。

虽然执行此函数时，获取到的这个返回值是可以遍历的，但它不能如正常数组一样，通过索引取某个元素。

要想「正常」使用，得转换成数组。

##### 示例

```shell
function test1() {

	local arr1=("cat" "dog" "duck" "cock" "fish" "goose")

	# 返回数组
	echo ${arr1[@]}
}

# 执行函数test1 并获取返回值
an_arr=$(test1)

# for a_temp in ${an_arr[@]}; do
# 	echo $a_temp
# done

# for a_temp in $an_arr; do
# 	echo $a_temp
# done

# 转换成数组
r_arr=($an_arr)
echo ${r_arr[@]}
```

---

## 常用命令

### 解析参数

在 Shell 中，三种方式解析命令行参数：

1. 直接处理，使用 `$1 $2` 这种特殊变量进行解析。
2. 使用 [getopts](#getopts)
3. 使用 [getopt](#getopt)

####  getopts

`getopts`，是 [Bash](#Bash) 的内置命令。

但 `getopts` 只能处理*短选项*，如 `-n` 这种。

#### getopt

`getopt` 不是标准的 unix 命令，但大多数 [Linux](Linux_Note.md) 发行版都自带了。

在非 [Bash](#Bash) 的 sh 中，没有 [getopts](#getopts)，所以可以使用 `getopt` 替代。

`getopt` 相较于 [getopts](#getopts) 有个优势，就是它可以处理*短选项*，也可以处理*长选项*，如 `--prefix=/home` 这种。

> [!tip] 
> 
> 在 [zsh](#zsh) 中，没有 `getopts`。
> 
> 而且 `getopts` 因为不能处理长选项，所以为了兼容性，还是优先使用 [getopt](#getopt) 命令。

`getopt` 老版本不太好用，后来的版本解决了老版本的问题，一般称为「增强版」。

，通过 `-T` 选项，即 `getopt -T`，就能检测当前发行版是否是「增强版」了。返回值为 4，就证明当前系统装的是增强版。

```shell
# silascript @ (base) in ~ [3:21:16] 
$ getopt -T
# silascript @ (base) in ~ [3:21:18] C:4
$ echo $?
4
```

---

## 相关工具

### shellcheck

[shellcheck](https://github.com/koalaman/shellcheck) 是一个 shell 的语法检查工具。

它是用 [haskell](https://www.haskell.org/) 写的，所以装它时得把 haskell 一并给装了。

```shell
pacman -S shellcheck
```

### shfmt

[shfmt](https://github.com/mvdan/sh) 是一款 shell 脚本格式化工具。

这工具可以与多款 [文本编辑器](../../Editors/Editors_Note.md) 的 shell 格式化插件配合使用。

> [!info] 
> 
> [各编辑器 shell 格式化插件列表](https://github.com/mvdan/sh#related-projects)

```shell
shfmt -l -w script.sh
```

#### 参数解释

`-i`：缩进设置。默认是 0，表示制表符缩进。大于 0 空格缩进，数字是就是空格数。
`bn`： && 及 |  另起一行
`ci`：switch case 缩进
`sr`：重定向符，`>`、`>>`、`<<` 这些，后面添加空格
`fn`：函数大括号，起始那个括号另起一行
`kp`：对齐

Google 风格：[Style guides for Google-originated open-source projects](https://google.github.io/styleguide/shellguide.html#indentation)

### json 相关工具

 #json

shell 下有多款 json 小工具：

* `jq` 或 `jshon`：shell 下的 JSON 解析器。
* `JSON.sh`、`jsonv.sh`：shell 脚本，能在 bash、zsh 等中解析 JSON。
* `JSON.awk`：JSON 解析器 awk 脚本。
* `json.tool`：python 模块。
* `undercore-cli`：基于 [NodeJS](../../Node/NodeJS_Note.md) 或 [JS](../../JS/JS_Note.md) 的 json 工具。

#### jq

 #shell #tools  #json #jq

安装 `jq`：

```shell
yay -S jq
```

#### 示例

##### 示例 1

```shell

# 从assets 数组中获取browser_download_url元素的值
# 过滤除了main.js manifest.json styles.css三个文件外所有文件
jq -r '.assets[] | .browser_download_url | select ( contains("main.js") or contains("manifest.json") or contains("styles.css") )' $json_path

```

> [!info] 
> 
> * `contains()` 方法是用来判断是否包含某字符串，包含返回 `true`，否则返回 `false`
> * `select()` 选择过滤数据

##### 示例 2

```shell
local tagstr="$2"
curl http://hub-mirror.c.163.com/v2/library/${image}/tags/list | jq --arg tstr $tagstr -r '.tags[]| select(contains($tstr))'
```

> [!info] 
> 
> * `jq --arg` 是定义变量的选项
> * `jq --arg tstr $tagstr`： `tstr` 为形参变量，是 jq 内部使用；而 `$tagstr` 是实参，外部传进来的。要使用形参时，使用 `$` 打头，跟普通 shell 变量使用一致。

---

## 相关笔记及资料

### 笔记

* [Linux笔记](Linux_Note.md)
* [Shell示例笔记](Shell_Example.md)
* [ZSH笔记](zsh_note.md) 

### 教程

* [Shell 编程范例 - 书栈网](https://www.bookstack.cn/read/open-shell-book/README.md)
* [Shell编程基础 - 书栈网](https://www.bookstack.cn/read/ShellBasicProgramming)

### 周边资料

* [awesome-shell](https://github.com/alebcay/awesome-shell)
* [awesome-shell_ZH-CN](https://github.com/xuxiaodong/awesome-shell/blob/master/README_ZH-CN.md#shell-%E5%8C%85%E7%AE%A1%E7%90%86)
* [Awesome Shell：命令行框架、工具包、指南清单（中译版） - Shell - 软件编程 - 深度开源](https://www.open-open.com/news/view/e54142)
* [shell echo 显示颜色 - 知乎](https://zhuanlan.zhihu.com/p/181609730)
* [实现shell脚本中的转圈、进度条等一些效果 · 艾莉亚的猫](https://yangtze736.github.io/%E6%8A%80%E6%9C%AF/2018/05/02/shell-tips/)

---

## 其他笔记

* [Shell 视频清单](Shell_Videos.md)
* [Shell 资料清单](Shell_Material.md)
* [Shell 示例](Shell_Example.md)
* [Linux 笔记](Linux_Note.md)
* [Linux 视频清单](Linux_Videos.md)

