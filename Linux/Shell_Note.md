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
modified: 2024-03-11 05:34:38
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

#### 相关资料

##### 资料链接

* [linux之登录式shell和非登录式shell - 知乎](https://zhuanlan.zhihu.com/p/415152910)
* [什么是 Linux 中的登录 Shell？ - Linux迷](https://www.linuxmi.com/linux-login-shell.html)
* [Linux文件 profile、bashrc、bash\_profile区别 - 知乎](https://zhuanlan.zhihu.com/p/405174594)
* [Linux 交互/非交互/登录/非登录 shell 的区别](https://blog.csdn.net/weixin_44648216/article/details/104056712)
* [shell 编程的，登录 shell 和非登录 shell 的区别是什么 ？- 知乎](https://www.zhihu.com/question/628721272)
* [千万别混淆 Bash/Zsh 的四种运行模式 - 知乎](https://zhuanlan.zhihu.com/p/47819029)

Sell 语法相关的内容请查看： [Shell 笔记](./Shell_Note.md)

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

方式 1：

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

#### 示例 2

```shell

# core_address是 denolehov/obsidian-git 这个样子。获取 / 左右两段字符串

# 取前段 使用从右向左取，取 / 最后一段
local account=${core_address%%/*}
# 取后牌戏 使用从左向右取，同样取 / 最后一段
local p_name=${core_address##*/}

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
> 跟 [Javascript](../JS/JS_Note.md) 等语言很像。

### <span id="shell_function_parameter">参数</span>

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

这工具可以与多款 [文本编辑器](../Editors/Editors_Note.md) 的 shell 格式化插件配合使用。

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

> [!note] 相关资料
> * [如何在 Linux 中使用 Shfmt 格式化 Shell 程序 – Digitalixy.com](https://digitalixy.com/linux/505871.html)
> * [使用 shfmt 更好地格式化 Shell 脚本 - Linux迷](https://www.linuxmi.com/shfmt-format-shell.html)
> * [Shell 代码规范：ShellCheck 与 shfmt 自动检查 - 知乎](https://zhuanlan.zhihu.com/p/408651010)
> * [如何在 Linux 中使用 Shfmt 格式化 Shell 程序 – Dbigr.com](https://dbigr.com/article/481199/) 
> * [格式化 Shell 脚本利器，轻松理解复杂代码-编写shell脚本的工具](https://www.51cto.com/article/705054.html)

### json 相关工具

 #json

shell 下有多款 json 小工具：

* `jq` 或 `jshon`：shell 下的 JSON 解析器。
* `JSON.sh`、`jsonv.sh`：shell 脚本，能在 bash、zsh 等中解析 JSON。
* `JSON.awk`：JSON 解析器 awk 脚本。
* `json.tool`：python 模块。
* `undercore-cli`：基于 [NodeJS](../Node/NodeJS_Note.md) 或 [JS](../JS/JS_Note.md) 的 json 工具。

#### jq

 #shell #shell/tools #shell/tools/jq

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

#### 相关资料

* [如何用 Linux 命令行工具解析和格式化输出 JSON - 知乎](https://zhuanlan.zhihu.com/p/77177160)
* [Shell：如何解析json - 知乎](https://zhuanlan.zhihu.com/p/675809200)
* [Linux 命令行工具之 jq 最佳实践 - 知乎](https://zhuanlan.zhihu.com/p/606945462)
* [Linux jq 命令讲解与实战操作（json字符串解析工具）- 博客园](https://www.cnblogs.com/liugp/p/17613011.html)
* [jq manual](https://jqlang.github.io/jq/manual/)
* [Linux shell jq工具操作文档（jq --help使用示例）-CSDN博客](https://blog.csdn.net/Dontla/article/details/135473677)

---

## 视频清单

* [2023年最通俗易懂的Shell教程](https://www.bilibili.com/video/BV1Tu411W7Up)
* [2023年最新Shell自动化开发全套顶级天花板教程](https://www.bilibili.com/video/BV1494y1i7ca)
* [Shell脚本从入门到实战](https://www.bilibili.com/video/BV1k94y1W7cJ)
* [B站第一的Shell脚本的学习方法，每天只需30分钟让你效率保障99倍](https://www.bilibili.com/video/BV1sT4y1b7si)
* [2024年最新Shell脚本顶级天花板教程，血赚！](https://www.bilibili.com/video/BV17S421P7fa)

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

