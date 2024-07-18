---
aliases: []
tags:
  - PL
  - lua
created: 2023-01-31 11:31:14
modified: 2024-07-18 11:11:37
---

# Lua 笔记

---

在 lua 文件顶行加上以下代码：
```lua
#!/usr/bin/lua
```
这样就能使用 `./xx.lua` 方式运行 lua 程序了。

> [!tip]
> `#!` 这个指令必须要顶行，不然会报错，执行不了。
> 
> `#!` 后面跟着的是 lua 调用的路径。可以使用 `whereis lua` 命令，看下当前系统 lua 装在哪。

---

## 变量

### 局部变量

lua 默认将所有变量注册在全局环境表中。默认除了三种情况会定义局部变量：

1. 使用 [local](#local) 关键字显示声明
2. `for` 循环控制语句中定义
3. [函数](#函数) 接收参数时定义

> [!info] 
> 
> 观察可知，除了使用 `local` 显式声明外，其他两种的「默认局部变量」本身从出生就自带有一种「临时性」特征，所以它们成为默认的局部变量是可以理解的。

#### 代码块

代码块，可以是一个控制结构体、一个函数体，或者一个 chunk。

所以，lua 中的「局部变量」与 [C](../C/C_Note.md) 不一样，它不仅仅指的是 [函数](#函数) 中的变量，它是在 [代码块](#代码块) 中定义的变量都称为「局部变量」。

#### local

`local` 关键字定义局部变量，这与 [Shell](../Linux/Shell_Note.md) 还有 [Vimscript 9](../vim/Vimscript9_Note.md) 的语法一样。

`local` 关键字不但能修饰变量，还能修饰 [函数](#函数)。

---

## 运算符

lua 大部分运算符都与 [C语言](../C/C_Note.md) 一样，只有少数运算符不一样。

lua 与 [C语言](../C/C_Note.md) 不同的运算符：

* `~=`：不等于
* `..`：连接运算符，如 `a .. b`，即是将 `a` 和 `b` 作为字符串连接起来。
* `^`：次方，如 `a^b`，即 `a` 的 `b` 次方。

---

## 类型

### table

table 类型是 lua 中强大的数据结构，也是唯一的数据结构，可以看成是数组或字典。

lua 使用 `{}` 来定义一个 table。

---

## 函数

---

## 问题

### 相关资料

* [Lua cURL如何获取JSON响应？ | Lua China - Lua 中国开发者社区](https://lua-china.com/posts/14460)

---

## 第三方库

### 简介

在 Lua 中使用第三方库的步骤如下：

1. 安装第三方库：

   * 如果是使用 [LuaRocks](#LuaRocks) 作为 [包管理器](#包管理器),可以通过 `luarocks install <library_name>` 命令安装第三方库。
   * 如果是手动下载第三方库,需要将库文件放在 Lua 的搜索路径中,通常是 `package.path` 和 `package.cpath` 指定的目录。

2. 在代码中引入第三方库：

   ```lua
   local library_name = require "library_name"
   ```

   这里的 `library_name` 是第三方库的名称,通常是库文件名去掉 `.lua` 后缀。

3. 使用第三方库提供的功能：

   ```lua
   local result = library_name.some_function(arg1, arg2)
   ```

   根据第三方库的文档,调用相应的函数或方法。

一些常见的 Lua 第三方库及其用途：

1. **LuaSocket**：提供网络编程功能,如 HTTP 客户端、服务器等。
2. **LuaSQL**：提供数据库访问功能,支持多种数据库。
3. **LPeg**：提供强大的正则表达式功能。
4. **Luvit**：基于 Lua 的异步 I/O 框架。
5. **Moonscript**：一种基于 Lua 的编程语言,提供更高级的语法糖。
6. **Penlight**：提供常用的工具函数,如文件操作、字符串处理等。
7. **Busted**：单元测试框架。
8. **LuaJIT**：高性能的 Lua 解释器。

使用第三方库可以大大提高开发效率,但需要注意库的版本兼容性和文档的完整性。同时也要注意库的安全性,避免引入安全隐患。

### 包管理器

#### 简介

Lua 有几种常见的包管理器：

1. **LuaRocks**：
   * [LuaRocks](#LuaRocks) 是 Lua 生态系统中最流行的包管理器。
   * 它提供了一个中央软件仓库,用于发布和安装 Lua 模块。
   * 使用 [LuaRocks](#LuaRocks) 可以方便地安装、升级和卸载 Lua 包。
   * 官网：[luarocks.org](https://luarocks.org/)

2. **Luadist**：
   * Luadist 是另一个流行的 Lua 包管理器。
   * 它提供了与 LuaRocks 类似的功能,但更加注重跨平台兼容性。
   * Luadist 可以在 Windows、macOS 和 Linux 上使用。
   * 官网: https://github.com/LuaDist/LuaDist

3. **Luvit**：
   * Luvit 是一个基于 Lua 的异步 I/O 框架,它也包含了一个包管理器。
   * Luvit 的包管理器与 npm 类似,提供了丰富的第三方库。
   * 官网：[luvit.io](https://luvit.io/)

4. **Moonrocks**：
   * Moonrocks 是 Moonscript 编程语言的包管理器。
   * Moonscript 是一种基于 Lua 的编程语言,Moonrocks 提供了 Moonscript 模块的安装和管理。
   * 官网：[moonrocks.moonscript.org](https://moonrocks.moonscript.org/)

使用包管理器的好处包括：

1. 方便安装和管理第三方库。
2. 自动处理依赖关系。
3. 提供中央软件仓库,方便发现和共享模块。
4. 支持版本管理,方便升级和回滚。

在实际开发中,根据项目需求和开发环境,选择合适的 Lua 包管理器使用。[LuaRocks](#LuaRocks) 作为 Lua 生态系统中最流行的包管理器,通常是首选。

#### LuaRocks

   [LuaRocks](https://luarocks.org/) 是 Lua 生态中最流行的包管理工具。

### json

在 Lua 中解析 JSON 文件有几种常见的方法：

1. 使用 Lua 标准库中的 `dkjson` 模块:

```lua
local json = require "dkjson"
local file = io.open("example.json", "r")
local data = file:read("*a")
file:close()
local result = json.decode(data)
```

2. 使用第三方库 `luajson`:

```lua
local json = require "luajson"
local file = io.open("example.json", "r")
local data = file:read("*a")
file:close()
local result = json.decode(data)
```

3. 使用第三方库 `lpack`:

```lua
local lpack = require "lpack"
local file = io.open("example.json", "r")
local data = file:read("*a")
file:close()
local result = lpack.unpack(data)
```

4. 使用第三方库 `json.lua`:

```lua
local json = require "json"
local file = io.open("example.json", "r")
local data = file:read("*a")
file:close()
local result = json.decode(data)
```

无论使用哪种方法,基本流程都是:

1. 打开 JSON 文件并读取其内容
2. 使用相应的 JSON 解析库将字符串数据转换为 Lua 表
3. 关闭文件

需要注意的是,不同的 JSON 解析库可能有不同的使用方式和功能,建议根据具体需求选择合适的库。

### 相关资料

* [Lua 包管理工具 Luarocks 详解](https://segmentfault.com/a/1190000003920034)

---

## 相关链接

* [Lua China - Lua 中国开发者社区](https://lua-china.com)

---

## 其他笔记

* [Lua视频清单](Lua_Videos.md)

