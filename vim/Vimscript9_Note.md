---
aliases: []
tags:
  - vim
  - vimscript
  - vim9
created: 2024-03-14 05:29:40
modified: 2024-03-19 23:20:47
---

# vimscript9 笔记

---

## 目录

* [函数](#函数)

---

## <span id="viml9_def">函数</span>

vim9 新语法，使用 `def` 来定义函数。并且与 [老语法的函数](Vimscript_Note.md#函数) `function` 不兼容。

```vim
def 函数名(参数1: 参数类型): 返回值类型
...
endef
```

---

## 改造老脚本

### 全面更换成新语法

1. 注释使用 `#` 代替老的 `"`
2. 使用 `vim9script` 标识当前脚本使用 vim9 语法
3. 使用 `import` 来代替 `source`
> [!info] 
> 
> `import "xxx/xxx/xx.vim"`
3. 使用 `def` 来定义 [函数](函数)
4. 函数定义参数及返回值加显示类型声明
5.kk 使用 `var` 代替 `let`，并把除了全局 `g` 外的变量修饰符去掉。
6. 赋值 `=` 两边加空格
7. 函数调用使用 `.`

示例：

```vim
vim9script
# 当前脚本是使用vim9的新脚本

# 导入函数
import "~/.vim/configs/commands/commands_basic.vim"

var nerdcommc_result = commands_basic.ExistPlug('preservim/nerdcommenter')

```

### 老脚本中使用 vim9 函数

如果当前脚本有特殊原因不得不还使用老的脚本，但又想使用 vim9 语法定义的函数，得作以下一些「改造」。

1. `import` 代替 `source`，老脚本同样能使用 `import`，而且使用新语法定义的函数，在使用时都得「导入」。
2. 在调用函数时，使用 `s:xxx.函数()` 方式。不得再使用原来的 `xxx#函数()` 了。

> [!info] 相关资料
> 
> [VIM 中文帮助: vim9 老式 Vim 脚本中导入 ](https://www.topbyte.cn/vimdoc/doc/vim9.html#fast-functions)

示例：

```vim

" 当前脚本仍是老脚本

" 导入脚本
import "~/.vim/configs/commands/commands_basic.vim"
" 调用ExisPlug函数
" ExisPlug函数是在commands_basic.vim文件中，使用vim9语法定义的
let s:lightline_result = s:commands_basic.ExistPlug('itchyny/lightline.vim')

```

---

## 相关资料

* [VIM 中文帮助: 使用 Vim9 脚本](https://yianwillis.github.io/vimcdoc/doc/vim9.html#vim9.txt)
* [VIM 中文帮助: 使用 Vim9 脚本 import](https://yianwillis.github.io/vimcdoc/doc/vim9.html#:import)

---

## 相关笔记

* [Vim笔记](Vim_Note.md)
* [Vimscript笔记](Vimscript_Note.md)