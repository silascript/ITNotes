---
aliases: []
tags: []
created: 2023-08-18 19:44:52
modified: 2024-03-14 05:57:27
---
# vimscript 笔记
---

## 目录

* [基本语法](#viml_basic)
  * [比较](#viml_basic_compare)
  * [作用域](#viml_basic_scope)

* [函数](#viml_func)
* [自定义命令](#viml_command)
* [常用函数](#viml_comuse_functions)
   * [字符串相关函数](#viml_comuse_functions_string)
   * [文件相关函数](#viml_comuse_functions_file)

---

## <span id="viml_basic">基本语法</span>

### <span id="viml_basic_compare">比较</span>

其他比较符跟其他语言一样。比较奇怪的是 `==` 这个比较符。

#### 三种 `==`

`==`：大小写是否敏感是根据用户设置而定

`==?`： 大小写不敏感

`==#`：大小写敏感

用来比较数字时，`==?` 或 `==#` 都可以，但如果用来比较字符串时，就应该谨慎。

---

### <span id="viml_basic_scope">作用域</span>

* g: 全局变量
* l: 局部变量
* s: 脚本变量
* a: 参数变量
* b: Buffer 变量

## <span id="viml_func">函数</span>

函数定义语法：

```vim

	function 函数名 (参数列表)	
	
	endfunction

```

## <span id="viml_command">自定义命令</span>

命令定义语法：

`command -nargs=* 命令 call 函数`

| 参数 | 定义 |
| :---: | :---: |
| `nargs=0` | 不接受任何参数（默认）|
| `-nagrs=1` | 只接受一个参数 |
| `-nargs=*` | 可接收任意个数参数 |
| `-nargs=?` | 可接受 1 个或者 0 个参数 |
| `-nargs=+` | 至少提供一个参数 |

参数类型：

### args

> `<args>`	命令的参数，和实际提供的完全相同 (但正如上面提到过的，数量或寄
>    存器会消耗若干参数，它们不再是 `<args>` 的一部分)。  
> `<lt>`	一个单独的 `<` (小于号) 字符。在扩展中可用于使以上转义序列按本义出现。

#### q-args

> 如果一个转义序列的最前两个字符是 `q-` (例如，`<q-args>`) 那么该值用引号括起，使
> 之在表达式里使用时成为合法的值。这种方式把参数当做单个值。如果没有参数，
> `<q-args>` 是空字符串。

#### f-args

> 要允许命令把参数传送给用户定义的函数，有一种特殊的形式 `<f-args> ("function args"，函数参数)`。它在空格和制表处分割命令行参数，每个参数分别用引号括起，然后把 `<f-args>` 序列替换为括起参数用逗号分隔的列表。参考下面的 Mycmd 示例。没有参数时，`<f-args>` 被删除。  要在 `<f-args>` 的参数中嵌入空白字符，在前面加上反斜杠。`<f-args>` 把每对反斜杠 （`\`） 用单个反斜杠替代。反斜杠后如跟非空白或反斜杠字符，保持不变。

## <span id="viml_comuse_functions">常用函数</span>

### <span id="viml_comuse_functions_string">字符串相关函数</span>

#### string()

> string({expr})	返回 {expr} 转换后的字符串。如果 {expr} 为数值、浮点数、字符
> 		串、blob 或它们的复合形式，那么用 |eval()| 可以把结果转回去。
> 			{expr} 类型	返回值 ~  
> 			字符串		'string' (单引号加倍)  
> 			数值		123  
> 			浮点数		123.123456 或 1.23456e8  
> 			函数引用	function('name')  
> 			blob		0z00112233.44556677.8899  
> 			列表		[item, item]  
> 			字典		{key: value, key: value}  
> 
> 		|List| 或 |Dictionary| 中如有递归引用，被替换为 "[...]" 或
> 		"{...}"。在此结果上运行 eval() 会出错。
> 
> 		也可用作 |method|: >
> 			mylist->string()

#### strlen()

> strlen({expr})	返回数值，即字符串 {expr} 的字节长度。  
>		如果参数为数值，先把它转化为字符串。其它类型报错。  
>		要计算多字节字符的数目，可用 |strchars()|。  
>		另见 |len()|、|strdisplaywidth()| 和 |strwidth()|。  
>		也可用作 |method|: > GetString()->strlen()

#### stridx()

> stridx({haystack}, {needle} [, {start}])		*stridx()*
>		返回数值，给出字符串 {haystack} 里第一个字符串 {needle} 出现的
>		字节位置。  
>		如果给出 {start}，搜索从 {start} 位置开始。可用来寻找第二个匹
>		配: >  
>			:let colon1 = stridx(line, ":")  
>			:let colon2 = stridx(line, ":", colon1 + 1)  
>		搜索对大小写敏感。  
>		如果 {needle} 不出现在 {haystack} 里，返回 -1。

#### strpart()

> strpart({src}, {start} [, {len} [, {chars}]])			*strpart()*
>		返回字符串，{src} 从第 {start} 个字节开始字节长度为 {len} 的子
>		串。  
>		{chars} 存在且为 TRUE 时，{len} 为字符位置的个数 (组合字符不单
>		独统计，因此 "1" 代表一个基本字符并可后跟任意的组合字符)。  
>		要以字符而不是字节计算 {start}，用 |strcharpart()|。  
>
>		如果选择不存在的字节，不会产生错误。只是那些字节被忽略而已。  
>		如果没有提供 {len}，子串从 {start} 开始直到 {src} 的结尾。 >
>			strpart("abcdefg", 3, 2)    == "de"
>			strpart("abcdefg", -2, 4)   == "ab"
>			strpart("abcdefg", 5, 4)    == "fg"
>			strpart("abcdefg", 3)       == "defg"
>
><		注意: 要得到第一个字符，{start} 必须是零。比如，要得到光标所在
>		的字符: >
>			strpart(getline("."), col(".") - 1, 1, v:true)
><
>		也可用作 |method|: >
>			GetText()->strpart(5)

### <span id="viml_comuse_functions_file">文件相关函数</span>

#### expand()

> `expand({expr} [, {nosuf} [, {list}]])`				*expand()*
> 		扩展 {expr} 里的通配符和下列特殊关键字。
> 		'wildignorecase' 此处适用。  
> 
> 		如果给出 {list} 且为 |TRUE|，返回列表。否则返回的是字符串，且
> 		如果返回多个匹配，以 `<NL>` 字符分隔 [备注: 5.0 版本使用空格。但
> 		是文件名如果也包含空格就会有问题]。  
> 
> 		如果扩展失败，返回空字符串。如果 {expr} 以 '%'，'#' 或 '<' 开
> 		始，不返回不存在的文件名。详见下。
> 
> 		如果 {expr} 以 '%'、'#' 或 '<' 开始，以类似于
> 		|cmdline-special| 变量的方式扩展，包括相关的修饰符。这里是一个
> 		简短的小结:  
> 
> 			`%`		当前文件名  
> 			`#`		轮换文件名  
> 			`#n`		轮换文件名 n  
> 			`<cfile>`		光标所在的文件名  
> 			`<afile>`		自动命令文件名  
> 			`<abuf>`		自动命令缓冲区号 (以字符串形式出现！)  
> 			`<amatch>`	自动命令匹配的名字  
> 			`<cexpr>`		光标所在的 C 表达式  
> 			`<sfile>`		载入的脚本文件或函数名  
> 			`<slnum>`		载入脚本文件行号或函数行号  
> 			`<sflnum>`	脚本文件行号，即使在函数中亦然  
> 			`<SID>`		`<SNR>123_`  其中 "123" 是当前脚本 ID  
> 					|`<SID>`|  
> 			`<stack>`		调用栈  
> 			`<cword>`		光标所在的单词  
> 			`<cWORD>`		光标所在的字串 (WORD)  
> 			`<client>`	最近收到的消息的 {clientid}  
> 					|server2client()|  
> 		修饰符:  
> 			`:p`		扩展为完整的路径  
> 			`:h`		头部 (去掉最后一个部分)  
> 			`:t`		尾部 (只保留最后一个部分)  
> 			`:r`		根部 (去掉一个扩展名)  
> 			`:e`		只有扩展名  
> 
> 		例如:   
> 			`:let &tags = expand("%:p:h") . "/tags"`  
> 		注意 扩展 `%`、`#` 或者 `<` 开头的字符串的时候，其后的文本被忽  
> 		略。这样 _ 不 _ 行:   
> 			`:let doesntwork = expand("%:h.bak") `
>  		应该这样: ` :let doeswork = expand("%:h") . ".bak"`  
> 		还要 注意 扩展 `<cfile>` 和其它形式只能返回被引用的文件名，而
> 不会进一步扩展。如果 `<cfile>` 是 `~/.cshrc`，你需要执行另一个
> `expand()` 把 `~/` 扩展为主目录的路径: `:echo expand(expand(<cfile>))`  
>		变量名和其后的修饰符之间不能有空白。|fnamemodify()| 函数可以用
>		来修改普通的文件名。  
>
>		使用 `%` 或 `#` 但当前或轮换文件名没有定义的时候，使用空字符
>		串。在无名缓冲区使用 "%:p"  生成当前目录，后加一个 `/`。  
>
>		如果 {expr} 不以 `%`、`#` 或 `<` 开始，它以命令行上的文件名那
>		样被扩展。使用 'suffixes' 和 'wildignore'，除非给出可选的
>		{nosuf} 参数而且为 |TRUE|。  
>		这里可以有不存在的文件的名字。`**` 项目可以用来在目录树里查
>		找。例如，要寻找当前目录及其下目录的所有的 "README": `:echo expand("**/README")`  
>		`expand()` 也可用来扩展变量和只有外壳知道的环境变量。但这会很
>		慢，因为需要使用外壳才能进行扩展。见 |expr-env-expand|。  
>		扩展后的变量还是被当作文件名的列表处理。如果不能扩展环境变量，
>		保留其不变。这样，`:echo expand('$FOOBAR')` 返回的还是
>		"$FOOBAR"。  
>
>		|glob()| 说明如何找到存在的文件。|system()| 说明如何得到外部命
>		令的原始输出。  
>
>		也可用作 |method|: `Getpattern()->expand()`

示例：

```vimscript

" 获取当前文件绝对路径
let current_file_path=expand('%:p')

" 获取当前文件所处的目录的绝对路径
" :h 其实就是取了'头'，就是文件路径的头即为它所在目录
let current_file_dir_path = expand('%:p:h')


" 获取当前文件的后缀名
let cfilesub = expand('%:e')

" 只取文件名不要路径和后缀名
let cfilename = expand('%:t:r')

```

#### getfsize

> `getfsize({fname})`					*getfsize()*
> 		返回数值，文件 {fname} 以字节数计算的大小。
> 		如果 {fname} 是目录，返回 0。
> 		如果找不到文件 {fname}，返回 -1。
> 		如果 {fname} 文件过大，超出了 Vim 的数值的范围，返回 -2。
> 
> 		也可用作 |method|: >
> 			GetFilename()->getfsize()

示例：

```

" 检测字节数
" 通过检测给定路径所代表的"文件"的字节数，来判断路径的状态 
" 目录返回 0，找不到文件返回 -1，文件过大，超出了 vim 的数值的范围，返回 -2
let fsizeresult = getfsize(l:dirnamepath)

```

#### isdirectory

> [!info] 
> 
> isdirectory({directory})				*isdirectory()*
> 		返回数值，如果名为 {directory} 的目录存在，返回 |TRUE|。如果
> 		{directory} 不存在或者不是目录，返回 |FALSE|。{directory} 可以
> 		是任何表达式，最终用作字符串。  
> 
> 		也可用作 |method|: >
> 			GetName()->isdirectory()  
> 

---

## 相关笔记

* [Vim笔记](Vim_Note.md)
* [Vimscript9笔记](Vimscript9_Note.md)

